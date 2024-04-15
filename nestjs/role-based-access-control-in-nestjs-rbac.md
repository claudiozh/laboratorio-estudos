# 🔐 Role Based Access Control in NestJS (RBAC)

RBAC (Role Based Access Control) is applied in many modern web applications. There are some standard libraries/packages we can use to integrate RBAC into backend applications. The fact that it allows for role hierarchies and other cool features makes is a great choice for backend developers as it takes away a lot of the heavy work from the developer. Even though these packages are great in what they achieve, I found that they offer a lot more than I need. In this article, I wanted to share my own approach for implementing RBAC.

### Project Structure <a href="#id-9e10" id="id-9e10"></a>

```
/src
    /decorators
        roles.decorator.ts
    /enums
        role.enum.ts
    /guards
        auth.guard.ts
        role.guard.ts
    /modules
        # your modules go here
    /shared
        access-control.service.ts
        shared.module.ts
  
    
```

Here’s a breakdown of the project structure:

* The folder ‘**decorators’** contains a single decorator we will be utilizing to attach metadata to the handler methods in our controller classes. This metadata will be the role information. By adding in role metadata to each controller, NestJS will know what roles are required for each handler method.
* The folder **‘enums’** contains our role enum that defines all the available roles in our system.
* The folder **‘guards’** will have two guards. The first guard, auth.guard.ts, will read the JWT from request and attach the decoded token to the request object. This decoded token will especially be used for the second guard which is role.guard.ts and this guard is going to read the role from token which is the role that the user currently has. Role guard then look at the role required by the controller method and run some logic to determine whether the user is authorized or not.
* The folder **‘modules’** contains all the modules that our application might have. For demo purposes, I’ll be creating a sample controller with some dummy methods just for the purposes of demonstrating how to use the above-mentioned decorators.
* Lastly, the **‘shared’** folder contains all the shared services. In this case, there’s only one service which is access-control.service.ts and this service will perform all the authorization logic.

### Defining Roles <a href="#id-0742" id="id-0742"></a>

Now that we understand the folder structure let’s get right into it.

First of all we have the following role enum:

```
export enum Role {
  ADMIN = 'ADMIN',
  USER = 'USER',
  GUEST = 'CLIENT',
  MODERATOR = 'MODERATOR'
}
```

Imagine a social platform where there are users, guest accounts, admins and moderators. Here’s the desired role hierarchy:

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*-0Yo11SPR_2TvoIZXfVp_Q.png" alt="" height="660" width="700"><figcaption></figcaption></figure>

There we can see that we have two different hierarchies:

1. ADMIN > USER > GUEST\
   This indicates any action guest can take, user should also be able to perform these actions and probably some additional actions. Similarly admin can do anything that user and guest is authorized to do and some other admin specific stuff. But it’s not the other way around
2. ADMIN > MODERATOR\
   From here, we can tell admin is more privileged than moderator as well

### Access Control Logic <a href="#e697" id="e697"></a>

Here’s the access control logic that I have developed for simple role based access management:

```
import { Injectable } from '@nestjs/common';
import { Role } from 'src/enums/role.enum';

interface IsAuthorizedParams {
  currentRole: Role;
  requiredRole: Role;
}

@Injectable()
export class AccessContorlService {
  private hierarchies: Array<Map<string, number>> = [];
  private priority: number = 1;

  constructor() {
    this.buildRoles([Role.GUEST, Role.USER, Role.ADMIN]);
    this.buildRoles([Role.MODERATOR, Role.ADMIN]);
  }

  /**
   * The buildRoles method allows for creating a role hierarchy between specified set of roles.
   * Roles have to be specified from least privileged user to the most priviliged one
   * @param roles Array that contains list of roles
   */
  private buildRoles(roles: Role[]) {
    const hierarchy: Map<string, number> = new Map();
    roles.forEach((role) => {
      hierarchy.set(role, this.priority);
      this.priority++;
    });
    this.hierarchies.push(hierarchy);
  }

  public isAuthorized({ currentRole, requiredRole }: IsAuthorizedParams) {
    for (let hierarchy of this.hierarchies) {
      const priority = hierarchy.get(currentRole);
      const requiredPriority = hierarchy.get(requiredRole);
      if (priority && requiredPriority && priority >= requiredPriority) {
        return true;
      }
    }
    return false;
  }
}
```

This service class allows us to define different role hierarchies using the buildRoles method. The way we represent how privileged is a role is by assigning a number to it. The higher the number is, the more privileged the role will be. By maintaining an array of Map objects that map roles to their priority number, we determine which role has higher privileges.

As seen above, buildRoles method is private so it cannot be called from outside and it takes in an array where roles should be ordered **FROM LEAST PRIVILEGED TO MOST PRIVILEGED ONE**. We call buildRoles method twice in the constructor to build two different hierarchies.

Next up, we have a public method called isAuhtorized which can be called from outside. It takes two arguments, currentRole and requiredRole. When these two arguments are passed, it’ll look for every hierarchy and if it finds any where current role is at least as privileged as required role, it will return true indicating authorization is successful. If no such match was found, false will be returned. Next, we will inject this access control service in our guards to utilize it’s access control logic.

### Auth Guard <a href="#id-094a" id="id-094a"></a>

Implementing auth guard which extracts and verifies the json web token (JWT) and then it attaches that token to the request object if everything went successful. Here I wont dive deep into how JWT is implemented as it is out of scope for this article. However, NestJS has documented it really well. See the official NestJS docs:

[Documentation | NestJS - A progressive Node.js frameworkNest is a framework for building efficient, scalable Node.js server-side applications. It uses progressive JavaScript…docs.nestjs.com](https://docs.nestjs.com/security/authentication?source=post\_page-----15c15090e47d--------------------------------#jwt-token)

Here I’m assuming you already set the environment variable for the JWT secret and registered JWT module globally using that secret.

Here’s the implementation for the auth.guard.ts:

```
import {
  CanActivate,
  ExecutionContext,
  Injectable,
  UnauthorizedException,
} from '@nestjs/common';
import { JwtService } from '@nestjs/jwt';
import { Request } from 'express';

@Injectable()
export class AuthGuard implements CanActivate {
  constructor(private jwtService: JwtService) {}

  async canActivate(context: ExecutionContext): Promise<boolean> {
    const request = context.switchToHttp().getRequest();
    const token = this.extractTokenFromHeader(request);
    if (!token) {
      throw new UnauthorizedException();
    }
    try {
      const payload = await this.jwtService.verifyAsync(token, {
        secret: process.env.JWT_SECRET,
      });
      request['token'] = payload;
    } catch {
      throw new UnauthorizedException();
    }
    return true;
  }

  private extractTokenFromHeader(request: Request): string | undefined {
    const [type, token] = request.headers.authorization?.split(' ') ?? [];
    return type === 'Bearer' ? token : undefined;
  }
}
```

Here we are extracting the token and trying to verify the it using JWT\_SECRET environment variable as the secret key. If everything goes successful, the decoded token payload is added to the request object so that other classess (like role guard) can pick them up and build authorization logic on it.

### Roles Decorator <a href="#id-7e23" id="id-7e23"></a>

Before we dive into role guard, I want to take a moment to talk about how to attach metadata to handler methods. Metadata is a key value pair where the key is string and value can be any type of object. We’ll use the role metadata to see what roles are required for the specific handler method. In our case, the role metadata will have a key of ‘role’ and the value will be an array of role enum values. Here’s the decorator definition:

```
import { SetMetadata } from '@nestjs/common';
import { Role } from 'src/enums/role.enum';

export const ROLE_KEY = 'role';

export const Roles = (...role: Role[]) => SetMetadata(ROLE_KEY, role);
```

As we can see from the above code, we extract the key into a constant to avoid any potential typos and the value for that metadata key is an array of Role enum values (Role\[]). A sample metadata for a handler method would look like the following:

```
{ "role": [Role.MODERATOR, Role.USER] }
/*
  Yes, one controller might have more than one roles. In this case,
  if the user role is privilged enough for at least one of these roles,
  authorization will succeed.
*/
```

### Role Guard <a href="#id-5ab6" id="id-5ab6"></a>

Now that we have auth guard and roles decorator in place, it’s time to implement the role guard. Here’s the implementation:

```
import { CanActivate, ExecutionContext, Injectable } from '@nestjs/common';
import { Reflector } from '@nestjs/core';
import { Observable } from 'rxjs';
import { ROLE_KEY } from 'src/decorators/roles.decorator';
import { Role } from 'src/enums/role.enum';
import { AccessContorlService } from 'src/shared/access-control.service';

export class TokenDto {
  id: number;
  role: Role;
}

@Injectable()
export class RoleGuard implements CanActivate {
  constructor(
    private reflector: Reflector,
    private accessControlService: AccessContorlService,
  ) {}

  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    const requiredRoles = this.reflector.getAllAndOverride<Role[]>(ROLE_KEY, [
      context.getHandler(),
      context.getClass(),
    ]);

    const request = context.switchToHttp().getRequest();
    const token = request['token'] as TokenDto;

    for (let role of requiredRoles) {
      const result = this.accessControlService.isAuthorized({
        requiredRole: role,
        currentRole: token.role,
      });

      if (result) {
        return true;
      }
    }

    return false;
  }
}
```

As seen above, we inject reflector to retrieve the metadata information about the handler method and we also inject access control serice to check for authorization. We extract the role from the request object (remember the decoded token was attached to the request object) and check that role against the list of roles required by handler method that we obtained using reflector. If there’s one role in handler metadata that our role is privileged enough compared to it, authorization will succeed.

### Applying RBAC on Handler Methods <a href="#a28c" id="a28c"></a>

Now that we have everything ready, it’s time to apply the above logic into an actual controller class. Assume that we have the following controller:

```
import {Controller, Get, UseGuards} from '@nestjs/common';
import {Role} from 'src/enums/role.enum';
import {Roles} from 'src/decorators/roles.decorator';

@Controller()
export class TestController {

  @Get('admin')
  @Roles(Role.ADMIN) // attaching metadata
  @UseGuards(AuthGuard, RoleGuard) // implementing the guards
  async adminOnlyEndpoint() {
    return "Welcome admin";
  }

  @Get('user-moderator')
  @Roles(Role.USER, Role.MODERATOR) // Both users and moderators can access this handler 
  @UseGuards(AuthGuard, RoleGuard)  // Of course, admin can also access this endpoint as admin has higher privelege than both
  async userModeratorEndpoint() {
    return "Welcome user or moderator";
  }
}
```

We can see that admin only endpoint requries the role to be admin so only admins can access it. The second method requires the role to be **EITHER** user **OR** moderator to access that endpoint. Guest would not be able to access this resource but admin obviously can because admin is has even higher privileges than both.

### Conclusion <a href="#id-38aa" id="id-38aa"></a>

To wrap up, even though some other access control libraries out there might be more capable and offer more features, in cases where you don’t want all those advanced features and implement something relatively simple, this approach might be helpful. I hope it was useful and happy coding 🙂
