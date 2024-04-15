# 😄 Como usar expressões regulares no caminho da rota e como tornar os parâmetros opcionais

```json
@Get('/:cameraId([a-f0-9]{8}-?[a-f0-9]{4}-?4[a-f0-9]{3}-?[89ab][a-f0-9]{3}-?[a-f0-9]{12})')
public get(@Param('cameraId') cameraId: string): Promise<Camera> {
    //
}
```
