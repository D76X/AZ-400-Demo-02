#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.
#This is the multistage Docker file that is used by Visual Studio (2022)
#to build the Docker Image locally and deploy it as a running container 
#within the debug session. 
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
#COPY ["WebApp1/WebApp1.csproj", "WebApp1/"]
COPY ["WebApp1.csproj", "WebApp1/"]
#RUN dotnet restore "WebApp1/WebApp1.csproj"
RUN dotnet restore "WebApp1.csproj"
COPY . .
WORKDIR "/src/WebApp1"
RUN dotnet build "WebApp1.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebApp1.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApp1.dll"]