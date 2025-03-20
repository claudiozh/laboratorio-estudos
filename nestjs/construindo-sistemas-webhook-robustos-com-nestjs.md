---
icon: key
---

# Construindo sistemas webhook robustos com NestJS

> Quer tornar seus webhooks sólidos como uma rocha? 💪 Aprenda a construir um sistema de webhook seguro e com capacidade de repetição com NestJS! 🚀

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*mrGBMPB5MOPyIm-hjUtzDA.png" alt="" height="395" width="700"><figcaption></figcaption></figure>

### O que são Webhooks? <a href="#id-1a47" id="id-1a47"></a>

Webhooks são mensagens automatizadas enviadas de um aplicativo para outro quando algo acontece. Pense neles como APIs orientadas a eventos que notificam outros sistemas sobre eventos específicos em tempo real. Por exemplo, quando um usuário faz um pagamento, um webhook pode notificar seu aplicativo para atualizar sua assinatura.

Embora os webhooks sejam poderosos, eles trazem desafios como garantir entrega confiável e proteger a troca de dados. Vamos explorar como lidar com isso em um aplicativo NestJS.

## Principais desafios em sistemas Webhook <a href="#id-581f" id="id-581f"></a>

1. **Confiabilidade de entrega:** O que acontece se o sistema de recebimento estiver inativo? Seu webhook deve lidar com as novas tentativas graciosamente.
2. **Segurança de Dados:** Webhooks frequentemente transmitem dados sensíveis. Garantir a segurança não é negociável.

## Construindo um sistema de webhook robusto em NestJS <a href="#id-238d" id="id-238d"></a>

Veja como você pode criar um sistema de webhook robusto no NestJS com lógica de repetição e segurança aprimorada.

## Etapa 1: configure seu projeto NestJS <a href="#afef" id="afef"></a>

Comece criando um novo projeto NestJS:

```sh
nestjs new webhook-system  
cd webhook-system  
npm install axios crypto
```

## Etapa 2: Crie um controlador Webhook <a href="#dbc4" id="dbc4"></a>

É aqui que as solicitações de webhook recebidas serão recebidas.

```typescript
import { Controller, Post, Req, Res, HttpStatus } from '@nestjs/common';  
import { Request, Response } from 'express';  

@Controller('webhooks')  
export class WebhookController {  

  @Post()  
  handleWebhook(@Req() req: Request, @Res() res: Response) {  
    // Process the webhook payload  
    console.log('Webhook received:', req.body);  

    // Acknowledge the request  
    res.status(HttpStatus.OK).json({ message: 'Webhook received' });  
  }  
}
```

## Etapa 3: Validar solicitações de webhook para segurança <a href="#id-0284" id="id-0284"></a>

Para garantir que apenas solicitações legítimas cheguem ao seu endpoint de webhook, use o HMAC (Hash-based Message Authentication Code) para verificar a integridade da solicitação.

### Gerar e verificar assinaturas <a href="#a971" id="a971"></a>

**Etapa 3.1: Adicionar lógica de validação de assinatura**

{% code overflow="wrap" %}
```typescript
import * as crypto from 'crypto';  

function verifySignature(payload: string, signature: string, secret: string): boolean {  
  const hash = crypto  
    .createHmac('sha256', secret)  
    .update(payload)  
    .digest('hex');  

  return hash === signature;  
}
```
{% endcode %}

### Etapa 3.2: Integrar a validação no controlador <a href="#d52b" id="d52b"></a>

{% code overflow="wrap" %}
```typescript
const secret = 'your-secret-key';  

@Post()  
handleWebhook(@Req() req: Request, @Res() res: Response) {  
  const payload = JSON.stringify(req.body);  
  const signature = req.headers['x-signature'] as string;  

  if (!verifySignature(payload, signature, secret)) {  
    return res.status(HttpStatus.UNAUTHORIZED).json({ message: 'Invalid signature' });  
  }  

  console.log('Valid webhook received:', req.body);  
  res.status(HttpStatus.OK).json({ message: 'Webhook received' });  
}
```
{% endcode %}

## Etapa 4: Implementar lógica de repetição <a href="#id-671b" id="id-671b"></a>

Às vezes, o sistema de recebimento pode falhar ao processar o webhook. Em tais casos, a lógica de repetição garante uma entrega confiável.

### Etapa 4.1: Use uma fila de repetição <a href="#id-71e3" id="id-71e3"></a>

Instalar `bull`para gerenciamento de filas.

```sh
npm install @nestjs/bull bull
```

### **Etapa 4.2: Crie uma fila de espera para novas tentativas** <a href="#id-3f7a" id="id-3f7a"></a>

```typescript
import { Injectable } from '@nestjs/common';  
import { InjectQueue } from '@nestjs/bull';  
import { Queue } from 'bull';  

@Injectable()  
export class WebhookService {  
  constructor(@InjectQueue('webhookQueue') private readonly webhookQueue: Queue) {}  

  async enqueueWebhook(payload: any) {  
    await this.webhookQueue.add('retry', payload, { attempts: 5, backoff: 1000 });  
  }  
}
```

### Etapa 4.3: Lidar com novas tentativas <a href="#e431" id="e431"></a>

```typescript
import { Processor, Process } from '@nestjs/bull';  
import { Job } from 'bull';  

@Processor('webhookQueue')  
export class WebhookProcessor {  
  @Process('retry')  
  async handleRetry(job: Job) {  
    console.log('Retrying webhook delivery:', job.data);  

    // Logic to resend the webhook payload  
    // For example, use axios to make an HTTP POST request  
  }  
}
```

## Etapa 5: Proteja o ponto de extremidade do Webhook com limitação de taxa <a href="#e3ab" id="e3ab"></a>

A limitação de taxa garante que seu ponto de extremidade do webhook não seja sobrecarregado por muitas solicitações, seja de forma maliciosa ou acidental.

### Instalar middleware de limitação de taxa <a href="#id-434b" id="id-434b"></a>

```sh
npm install @nestjs/throttler
```

### Adicionar lógica de limitação de taxa <a href="#a479" id="a479"></a>

```typescript
import { ThrottlerGuard } from '@nestjs/throttler';  
import { UseGuards } from '@nestjs/common';  

@Controller('webhooks')  
@UseGuards(ThrottlerGuard)  
export class WebhookController {  
  @Post()  
  handleWebhook(@Req() req: Request, @Res() res: Response) {  
    // Process the webhook  
  }  
}
```

## Etapa 6: Adicionar registro para depuração <a href="#id-33e8" id="id-33e8"></a>

Os logs são essenciais para monitorar e depurar problemas de webhook. Use os recursos de log integrados do NestJS ou uma biblioteca como `winston`.

### Exemplo: Usando Winston <a href="#id-230a" id="id-230a"></a>

```sh
npm install @nestjs/winston winston
```

```typescript
import { WinstonModule } from 'nest-winston';  
import * as winston from 'winston';  

@Module({  
  imports: [  
    WinstonModule.forRoot({  
      transports: [  
        new winston.transports.Console(),  
        new winston.transports.File({ filename: 'webhook.log' }),  
      ],  
    }),  
  ],  
})  
export class AppModule {}
```

## Etapa 7: Monitore seu sistema Webhook <a href="#id-5b98" id="id-5b98"></a>

Use ferramentas de monitoramento externas como Datadog ou New Relic para rastrear o desempenho e as falhas do webhook em tempo real.

## Conclusão <a href="#e84e" id="e84e"></a>

Webhooks são uma ferramenta poderosa, mas construir um sistema de webhook confiável e seguro requer planejamento cuidadoso. Ao implementar filas de repetição, proteger solicitações com HMAC, adicionar limitação de taxa e usar registro robusto, você pode garantir que seu sistema de webhook NestJS seja sólido como uma rocha.

Leve seu jogo de webhook para o próximo nível e dê ao seu aplicativo a confiabilidade e a segurança que ele merece! 🚀
