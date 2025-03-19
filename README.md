# notes-app

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### For BD create schema "notes_db" and use this to create table notes:
```
CREATE TABLE notes (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    content VARCHAR(255),
    x INT,
    top INT
);
```
Paste MySQL password to server/src/main/resources/application.properties
