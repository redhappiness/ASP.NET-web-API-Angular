# ASP.NET-web-API-Angular
This is a demo for creating a web application using ASP.NET web API and Anular
## Prerequisite
1. .NET SDK
- https://dotnet.microsoft.com/download
2. Visual Studio Code
- https://code.visualstudio.com/download
- Install C# for Visual Studio Code extension
3. Node.js
- https://nodejs.org/en/download/
4. Angular
```
npm install -g @angular/cli
```
6. Docker
## Steps
1. Create a ASP.NET + Angular template with dotnet CLI
```
dotnet new angular -o MyNewApp
cd MyNewApp
```
2. Build and run
```
dotnet build
dotnet run
```
3. Create a Dockerfile under MyNewApp folder
```
# https://hub.docker.com/_/microsoft-dotnet
FROM mcr.microsoft.com/dotnet/sdk:5.0.103 AS build
WORKDIR /build

RUN curl -sL https://deb.nodesource.com/setup_10.x |  bash -
RUN apt-get install -y nodejs

# copy csproj and restore as distinct layers
COPY ./*.csproj .
RUN dotnet restore

# copy everything else and build app
COPY . .
WORKDIR /build
RUN dotnet publish -c release -o published --no-cache

# final stage/image
FROM mcr.microsoft.com/dotnet/aspnet:5.0
WORKDIR /app
COPY --from=build /build/published ./
ENTRYPOINT ["dotnet", "MyNewApp.dll"]
```
4. Build Docker image and run container
```
docker build -t mynewapp .
docker run -d -p 8080:80 --name myApp mynewapp
```
5. Open https://localhost:8080
