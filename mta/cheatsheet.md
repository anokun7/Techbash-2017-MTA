docker image build -t signup-web:2.0 .
docker run -d -e ACCEPT_EULA=Y --env-file=db-credentials.env --name signup-db microsoft/mssql-server-windows-express
docker ps
docker rm -f signup-web
docker run -d -p 80:80 --env-file=db-credentials.env --name signup-web signup-web:2.0
docker inspect signup-web
# Submit the form
docker container exec signup-db powershell "Invoke-SqlCmd -Query 'Select * from Prospects' -Database Signup"
