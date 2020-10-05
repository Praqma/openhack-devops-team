# Base
FROM microsoft/dotnet:2.1-aspnetcore-runtime as base
EXPOSE 80
EXPOSE 443

# Build and Test
FROM microsoft/dotnet:2.1-sdk AS build
WORKDIR /app

COPY . ./

RUN dotnet build "web/poi.csproj"
RUN dotnet build "tests/UnitTests/UnitTests.csproj"
RUN dotnet test "tests/UnitTests/UnitTests.csproj" --logger "trx;LogFileName=poi.trx" 

# Publish
FROM build as publish
RUN dotnet publish "web/poi.csproj" -c Release -o out

# Final
FROM base as final
WORKDIR /app

ENV SQL_USER="YourUserName" \
SQL_PASSWORD="changeme" \
SQL_SERVER="changeme.database.windows.net" \
SQL_DBNAME="mydrivingDB" \
WEB_PORT="8080" \
WEB_SERVER_BASE_URI="http://0.0.0.0" \
ASPNETCORE_ENVIRONMENT="Production"

COPY --from=publish /app/web/out .

ENTRYPOINT ["dotnet", "poi.dll"]
