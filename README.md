# nestjs-with-apollo-federation-gateway

nestjs with apollo federation gateway to compose sub graphs from different microservices

This is a demo example to showcase how we can have a federated gateway composing all sub-graphs from different microsercices 
With this whole Demo we want to have different microservices exposing graphql schema and gateway is composing all to expose from single api endp-point

![](./snap/snap.png)

![](https://raw.githubusercontent.com/rkudryashov/graphql-federation/master/architecture.png)

We will build 

- nestjs graphql gateway service
- nestjs user auth service 
- nestjs user blog service
- nestjs user comment service

### Setting up the whole platform Locally 

This platform contains all these components 

- User Manegement service
- Home Manager service
- Gateway Service
- Home Manager Service

### Running all these services 

> we are using docker-compose to bootstrap all container only (database containers)
> in the root of the project run
```
docker-compose up
```
> check the longs and make sure databases has been created 
```
git clone <repo>
cd nestjs-with-apollo-federation-gateway
cd packages
```
### Running Auth service

```sh
cd auth-service
vi .env
```
update env with this content 
```
DATABASE_URL= postgres://api:development_pass@localhost:5431/auth-api
SENDGRID_API_KEY=SSSS
SENDGRID_VERIFIED_SENDER_EMAIL=ssss@gmail.com
DEBUG="ssss:*"
LOG_LEVEL=http
PORT=5006
NODE_ENV=local
JWT_SECRET=HELLO
JWT_EXPIRE_IN=3600*24
```
Now run application in watch mode it will be live on localhost:5006 
```sh
npm run start:dev
```
### Running Home Manager

```sh
cd home-manager
vi .env
```
update env with this content 
```
NODE_ENV=local
LOG_LEVEL=http
PORT=5003
SECRET_KEY=HELLO
NEW_RELIC_KEY=
DATABASE_URL=postgres://api:development_pass@localhost:5433/home-manager-api
```
Now run application in watch mode it will be live on localhost:5003 
```sh
npm run start:dev
```
docker-compose up
![](./snap/service-snap-3.png)

Auth Apis `npm run start:dev`
![](./snap/service-snap-4.png)

Home Apis `npm run start:dev`
![](./snap/service-snap-5.png)

For Gateway also trigger same command `npm run start:dev`
![](./snap/service-snap-6.png)

Now lets test our application 
Our Gateway is connected to both sub-graphq apis running on different ports 
We have to make sure both service are running before running gateway service 
```json
{ name: 'User', url: 'http://localhost:5006/graphql' },
{ name: 'Home', url: 'http://localhost:5003/graphql' },
```
If you are using same PORT and env then Gateway will run on `http://localhost:5002/graphql`

![](./snap/service-snap-2.png)
