## Initialize NestJS Project

```
npx @nestjs/cli new . --skip-git
```

Choose:

- Package manager: npm
- Strict mode: yes

---

## Install Core Dependencies (Minimal & Correct)

### Config & validation

```
npm install @nestjs/config
npm install class-validator class-transformer
```

### Database (TypeORM)

```
npm install @nestjs/typeorm typeorm pg
```

### Auth

```
npm install @nestjs/jwt @nestjs/passport passport passport-jwt
npm install bcrypt
npm install --save-dev @types/bcrypt @types/passport-jwt
```

### Utilities

```
npm install uuid
```

---
