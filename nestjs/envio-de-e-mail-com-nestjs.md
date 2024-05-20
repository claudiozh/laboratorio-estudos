# 📧 Envio de e-mail com NestJS

> Mastering Email Integration in NestJS with EJS Templates

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*ND-qJv7rWDqrisfV" alt="" height="467" width="700"><figcaption><p>Photo by <a href="https://unsplash.com/@agforl24?utm_source=medium&#x26;utm_medium=referral">Tai Bui</a> on <a href="https://unsplash.com/?utm_source=medium&#x26;utm_medium=referral">Unsplash</a></p></figcaption></figure>

Hello fellow developers! If you’ve been exploring the realms of JS, TS, and popular frameworks like I have, you’ll be excited about our topic today. NestJS has been a personal favorite of mine, and today, we’ll delve into one of its nifty features — sending emails.

By the end of this guide, you’ll not only understand the process of sending emails with NestJS, but you’ll also have an overview of crafting beautiful email templates using EJS. So, let’s dive in!

> _👉_ **How emergence of new tech changes the software development landscape:**

[Where Did Microservices Go?Reflection on Microservices and how new technology changes the landscape.differ.blog](https://differ.blog/p/where-did-microservices-go-794f4f?source=post\_page-----42e03ef1fb05--------------------------------)

## 1. Setting Up Our Nest Project 🛠️ <a href="#id-4c4e" id="id-4c4e"></a>

Start by creating a fresh NestJS project with the following commands:

```
nest new nestjs-email
cd nestjs-email
```

## 2. Installing the Necessary Packages 📦 <a href="#id-7c48" id="id-7c48"></a>

Before we embark on our email journey, let’s equip ourselves with the right tools.

```
npm i @nestjs-modules/mailer
```

And for our EJS templates:

```
npm i ejs
```

## 3. Setting Up Our Email Resource 💌 <a href="#id-1b69" id="id-1b69"></a>

To create the foundation for our emails, initiate the email resource:

```
nest g resource email
```

## 4. Configuring the Email Module 🖇️ <a href="#id-4eb7" id="id-4eb7"></a>

Hop into the email module, and update the imports section as follows:

```typescript
@Module({
  imports: [
    MailerModule.forRoot({
      transport: {
        host: '<host>',
        port: Number('<port>'),
        secure: false,
        auth: {
          user: '<username>',
          pass: '<password>',
        },
      },
      defaults: {
        from: '"From Name" <from@example.com>',
      },
      template: {
        dir: join(__dirname, 'templates'),
        adapter: new EjsAdapter(),
        options: {
          strict: true,
        },
      },
    }),
  ],
  controllers: [EmailController],
  providers: [EmailService],
})
```

Ensure you’ve imported the EjsAdapter:

```
import { EjsAdapter } from '@nestjs-modules/mailer/dist/adapters/ejs.adapter';
```

Do not forget to add this in `nest-cli.json`

```
{
  "collection": "@nestjs/schematics",
  "sourceRoot": "src",
  "compilerOptions": {
    "assets": ["mail/templates/**/*"], // Treat templates as assets
    "watchAssets": true // Enable real-time template copying
  }
}
```

## 5. Crafting the Email Sending Function 📤 <a href="#id-2795" id="id-2795"></a>

Lastly, within our service, we’ll implement a function to send the welcome email:

```typescript
import { MailerService } from '@nestjs-modules/mailer';

interface Email {
  to: string;
  data: any;
}

export class EmailService {

constructor(private readonly mailerService: MailerService) {}

  async welcomeEmail(data) {
    const { email, name } = data;

    const subject = `Welcome to Company: ${name}`;

    await this.mailerService.sendMail({
      to: email,
      subject,
      template: './welcome',
      context: {
        name,
      },
    });
  }
}
```

Note: `welcome` refers to the `welcome.ejs` template.

You can stop here and send the email by directly calling the `welcomeEmail` function on the service. However, if you're like me and you appreciate the beauty of a more robust and modular system, let's dive a little deeper and introduce the event emitter in our application.

## 6. Triggering the Email Using Event Emitters 🚀 <a href="#id-3494" id="id-3494"></a>

Integrate an event-driven approach to make our application even more robust. Let’s say we want the welcome email to be sent out when a new user registers. We can achieve this by emitting an event post-registration and having our `EmailService` listen to that event.

First install the required package:

```
npm i --save @nestjs/event-emitter
```

Don't forget to import the `EventEmitterModule` in `app.module.ts`

```typescript
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { EmailModule } from './modules/email/email.module';
import { UserModule } from './modules/user/user.module';
import { EventEmitterModule } from '@nestjs/event-emitter';

@Module({
  imports: [
    EmailModule,
    UserModule,
    EventEmitterModule.forRoot()
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

The create a module called `user`

```
nest g resource user
```

In `user.controller.ts` :

```typescript
import { Body, Controller, Post } from '@nestjs/common';
import { TypedEventEmitter } from '@/event-emitter/typed-event-emitter.class';

@Controller('user')
export class UserController {
  constructor(private readonly eventEmitter: TypedEventEmitter) {}

  @Post('sign-up')
  async create(@Body() body) {
    this.eventEmitter.emit('user.welcome', {
      name: 'Bhagyajit Jagdev',
      email: body.email,
    });

    this.eventEmitter.emit('user.verify-email', {
      name: 'Bhagyajit Jagdev',
      email: body.email,
      otp: '****', // generate a random OTP
    });

    // Add your user to the database
  }
}
```

Next, in your `EmailService`, listen for those event:

```typescript
import { EventPayloads } from '@/interface/event-types.interface';
import { MailerService } from '@nestjs-modules/mailer';
import { Injectable } from '@nestjs/common';
import { OnEvent } from '@nestjs/event-emitter';

@Injectable()
export class EmailService {
  constructor(private readonly mailerService: MailerService) {}

  @OnEvent('user.welcome')
  async welcomeEmail(data: EventPayloads['user.welcome']) {
    const { email, name } = data;

    const subject = `Welcome to Company: ${name}`;

    await this.mailerService.sendMail({
      to: email,
      subject,
      template: './welcome',
      context: {
        name,
      },
    });
  }

  @OnEvent('user.reset-password')
  async forgotPasswordEmail(data: EventPayloads['user.reset-password']) {
    const { name, email, link } = data;

    const subject = `Company: Reset Password`;

    await this.mailerService.sendMail({
      to: email,
      subject,
      template: './forgot-password',
      context: {
        link,
        name,
      },
    });
  }

  @OnEvent('user.verify-email')
  async verifyEmail(data: EventPayloads['user.verify-email']) {
    const { name, email, otp } = data;

    const subject = `Company: OTP To Verify Email`;

    await this.mailerService.sendMail({
      to: email,
      subject,
      template: './verify-email',
      context: {
        otp,
        name,
      },
    });
  }
}
```

This event-driven approach keeps our application modular, with each segment focused on its core function.

## 7. Type Checking Event Emitter Payload <a href="#id-4a4d" id="id-4a4d"></a>

To make the event payload type safe we can create a wrapper around the `EventEmitter2` :

```typescript
// src\event-emitter\typed-event-emitter.class.ts

import { Injectable } from '@nestjs/common';
import { EventPayloads } from '@/interface/event-types.interface';
import { EventEmitter2 } from '@nestjs/event-emitter';

@Injectable()
export class TypedEventEmitter {
  constructor(private readonly eventEmitter: EventEmitter2) {}

  emit<K extends keyof EventPayloads>(
    event: K,
    payload: EventPayloads[K],
  ): boolean {
    return this.eventEmitter.emit(event, payload);
  }
}
```

Then create a module and make it `@Global`\
make sure to import `TypedEventEmitterModule` in `app.module.ts` imports

```typescript
import { Global, Module } from '@nestjs/common';
import { TypedEventEmitter } from './typed-event-emitter.class';

@Global()
@Module({
  providers: [TypedEventEmitter],
  exports: [TypedEventEmitter],
})
export class TypedEventEmitterModule {}
```

Then import it in the `user.controller.ts` :

```typescript
export class UserController {
  constructor(private readonly eventEmitter: TypedEventEmitter) {}
}
```

Now you can create an interface for all the payload types and type safe it:

```typescript
// src\interface\event-types.interface.ts
export interface EventPayloads {
  'user.welcome': { name: string; email: string };
  'user.reset-password': { name: string; email: string; link: string };
  'user.verify-email': { name: string; email: string; otp: string };
}
```

## Conclusion 🌟 <a href="#ce1c" id="ce1c"></a>

And there you have it! With these steps, you’re now equipped to send beautifully templated emails using NestJS and EJS. Plus, with the power of event emitters, you can ensure your application is reactive and modular. As you integrate this into your projects, I believe you’ll find it to be a game-changer. For more insights into web development and other intriguing topics, feel free to explore my previous works over at [my Medium profile](https://medium.com/@bhagyajit).

GitHub Code: Find the working code on my [GitHub repository.](https://github.com/bhagyajitjagdev/nestjs-email) Feel free to explore, fork, and adapt it to your needs.

Happy coding! 🚀
