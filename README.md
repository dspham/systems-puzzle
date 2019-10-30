# Insight DevOps Engineering Systems Puzzle

## My solution for [Insight's systems puzzle](https://github.com/InsightDataScience/systems-puzzle)

### To Run:
```
docker-compose up -d db

docker-compose run --rm flaskapp /bin/bash -c "cd /opt/services/flaskapp/src && python -c  'import database; database.init_db()'"\n

docker-compose up -d
```

### Steps taken:

1. Remapped Jenkins in my local environment to 7070 to free port 8080.
2. Container now runs, but app is not loading. 
3. Reviewed scripts within the app. Checked the docker-compose file and saw that the ports were switched. Changed the ports so host is at 8080 and container 80.
4. 502 bad gateway error from nginx still. Checked the logs of the container. Noticed that the port for the development server is at 5000. Changed proxy pass in nginx flask config to 5000. 
5. Dockerfile shows port 5001, but it’s not accessible. Opened up port in app.py to get it exposed. 
6. Double checked the app and db is still not working and leads to a broken route (http://localhost%2Clocalhost:8080/success). Trying the success route itself gives an empty object. Need to first redirect to success route on post. 
7. Reviewed proxy setups in nginx config. Server name localhost was labeled twice in the url and in turn host is labelled twice in the proxies. Removed one of the hosts. Restarted docker instances. Tested app again and now being redirected to correct success route with an object of [, , , , , ]. 
8. Looked over app.py and database.py again. Db and add_method both look configured correctly. Success method is not returning the query.
9. Fixed success method to display results of the query. 
10. App is now running. 