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



