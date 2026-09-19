If you mean **Docker `RUN` vs `CMD`** in a Dockerfile:

 | `RUN` | `CMD` |
| --- | --- |
| Executes **while building** the image | Executes **when starting** the container |
| Creates a new image layer | Provides the container's default command |
| Used to install/configure things | Used to run your application |
| Runs once during `docker build` | Runs each time the container starts |

### Example

```
FROM node:20

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

CMD ["npm", "start"]
```

 Here:

 - `RUN npm install` → runs during `docker build`
- `CMD ["npm", "start"]` → runs when you do `docker run`

 For example:

```
docker build -t myapp .
docker run myapp
```

 Think of it as:

 **`RUN` = build the image**\
 **`CMD` = run the container**

 Also, `RUN` and `CMD` are different from `ENTRYPOINT`: `ENTRYPOINT` is typically used when you want to make the container behave like a specific executable.
