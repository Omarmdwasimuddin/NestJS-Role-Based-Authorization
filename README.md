## Role-Based Authorization


>Create guard
>```bash
>nest g guard [name]
>```
>##
>Create guard with path
>```bash
>nest g guard guards/roles
>```
>---



><img width="244" height="67" alt="image" src="https://github.com/user-attachments/assets/ae5ccd7d-7b53-4fb7-b371-2dc0841ccbc7" />

### `roles.guard.ts`
```bash
import { CanActivate, ExecutionContext, Injectable } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class RolesGuard implements CanActivate {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    return true;
  }
}
```
---



> ## Add file- roles.decorator.ts & roles.enums.ts
> <img width="255" height="153" alt="image" src="https://github.com/user-attachments/assets/79a3fc7d-4956-4042-9efe-1dc83b45f6bf" />



### `roles.decorator.ts`
```bash
// Custom decorator
import { SetMetadata } from "@nestjs/common";

export const ROLES_KEY = 'roles';

export const Roles = (...roles: string[]) => SetMetadata(ROLES_KEY, roles);
```
### `roles.enums.ts`
```bash
export enum Role {
    User = 'user',
    Admin = 'admin'
}
```
---



### `roles.guard.ts`
```bash
import { CanActivate, ExecutionContext, Injectable } from '@nestjs/common';
import { Reflector } from '@nestjs/core';
import { Observable } from 'rxjs';
import { Role } from './roles.enums';
import { ROLES_KEY } from './roles.decorator';

@Injectable()
export class RolesGuard implements CanActivate {

  constructor(private reflector: Reflector) {}

  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    
    const requiredRoles = this.reflector.getAllAndOverride<Role[]>(
      ROLES_KEY,[
        context.getHandler(),
        context.getClass(),
      ]
    );
    if (!requiredRoles) return true;
    const request = context.switchToHttp().getRequest<{ headers: Record<string, string>}>();
    const userRole = request.headers['x-user-role'] as Role;
    return requiredRoles.includes(userRole);
  }
}
```
---



### ``
```bash

```
---
