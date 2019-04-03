#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/dotnet:2.2-aspnetcore-runtime-nanoserver-1803 AS base
WORKDIR /app
EXPOSE 80

FROM microsoft/dotnet:2.2-sdk-nanoserver-1803 AS build
WORKDIR /src
COPY ["Lister.Server.Core/Lister.Server.Core.csproj", "Lister.Server.Core/"]
RUN dotnet restore "Lister.Server.Core/Lister.Server.Core.csproj"
COPY . .
WORKDIR "/src/Lister.Server.Core"
RUN dotnet build "Lister.Server.Core.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "Lister.Server.Core.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Lister.Server.Core.dll"]