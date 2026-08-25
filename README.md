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


### Create controller
```bash
nest g controller user-roles
```
### `user-roles.controller.ts`
```bash
import { Controller, Get, UseGuards } from '@nestjs/common';
import { Roles } from 'src/guards/roles/roles.decorator';
import { Role } from 'src/guards/roles/roles.enums';
import { RolesGuard } from 'src/guards/roles/roles.guard';

@Controller('user-roles')
export class UserRolesController {

    @Get('admin-data')
    @UseGuards(RolesGuard)
    @Roles(Role.Admin)
    getAdminData(){
        return { message: "Only Admins can access this data" };
    }
    @Get('user-data')
    getUserData(){
        return { message: "Any authenticated user can access this data" };
    }

}
```
---



> ## OUTPUT
> <img width="700" height="525" alt="image" src="https://github.com/user-attachments/assets/099387bd-b898-4563-a580-37d2d95d6bc5" />
>
>##
> Note: headers e x-user-role dite hobe & value dite hobe ekhane value hishabe admin ache
>
> <img width="752" height="476" alt="image" src="https://github.com/user-attachments/assets/c394fd57-c403-435f-9141-2abcf15590d3" />
>
>##
> Note: jekono user ei access korte parbe
>
> <img width="752" height="492" alt="image" src="https://github.com/user-attachments/assets/953071e3-12a4-4ad1-be70-7c8a2194b829" />
>
>---
