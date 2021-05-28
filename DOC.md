# SUMMARY

The followings help to collect metrics over this repo on a weekly basis for the number of branches older than 7 days with a Cronjob or github-action-workflow which runs a bash-script and Graphite is used for collecting numbers weekly into a time-series database(statsD) and for visualizing them with Graphite and Grafana.

# Writing Script-Application

We need to create a cronjob that runs a script-application weekly which collect the number of branches older than 7 days.

NOTE: Consider `chmod +x branch_monitor.sh` if using ./filename.sh format

1. Create the bash script within the repo as **branch_monitor.sh** whcih collects branches older than 7 days and parses them into Graphite
2. The cronjob can be used in two different ways to run the needed bash script-Second way recommended!
   I. Use `crontab -e` commnad and paste `@weekly bash <fullpath>/branch_monitor.sh`
   II. Write a github-action-workflow within this repo which will run on a schedule(every week). Please check **./github/workflows/branch_monitor.yaml**

# Deploying Monitoring Stack(statsD/Graphite/Grafana)

![alt text](https://www.bogotobogo.com/DevOps/Docker/images/Docker-StatsD-Graphite/StatsD-Graphite-Diagram-with-Carbon-Whisper.png)

1. Let's start a Docker container which spins up StatsD, Graphite and Grafana

```
docker run -d\
 --name graphite\
 --restart=always\
 -p 80:80\
 -p 81:81\
 -p 2003-2004:2003-2004\
 -p 2023-2024:2023-2024\
 -p 8125:8125/udp\
 -p 8126:8126\
 hopsoft/graphite-statsd
```

2. Let's login at http://localhost:81/account/login with _root/root_ for Graphite: Then, we may want to update the admin user's profile at: http://localhost:81/admin/users/edit/1

3. Let's login at: http://localhost with _admin/admin_ for Grafana: Then, we may want to update the admin user's profile at: http://localhost/admin/users/edit/1

4. Connect Grafana to Graphite

NOTE: Grafana and graphite are separate applications. Grafana is for visualizing data, especially time series databases. Graphite is a time series databases. Grafana can also visualize data from InfluxDB, Prometheus, ElasticSearch, etc.

![alt text](https://www.bogotobogo.com/DevOps/Docker/images/Docker-StatsD-Graphite/Add-Data-Source.png)
