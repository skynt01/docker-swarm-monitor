# docker-swarm-monitor
simple configurations to run docker swarm monitor componentes including prometheus server and pre-configured scraping

## run
```bash
$ git clone https://github.com/opvizordz/docker-swarm-monitor.git
$ cd docker-swarm-monitor
docker stack deploy -c docker-compose.stack.yml docker-swarm-monitor
```

### check status
```bash
docker stack ls # show swarm stack deployments and status
docker service ls # show services
docker service logs -f <service> # check logs for debugging purpose
docker stack rm docker-swarm-monitor # remove the project
```

The following components are part of this project

* cAdvisor
* node Exporter
* dockerd-exporter
* Prometheus

## Étapes à suivre

Créer une swarm avec un master et un ou plusieur nodes.

Ouvrir les ports suivants pour pouvoir utilisé tous les services de la swarm ( réseaux interne et autres ) sur master et node :

```bash
firewall-cmd --add-port=2376/tcp --permanent
firewall-cmd --add-port=2377/tcp --permanent
firewall-cmd --add-port=7946/tcp --permanent
firewall-cmd --add-port=7946/udp --permanent
firewall-cmd --add-port=4789/udp --permanent
firewall-cmd --add-port=9323/tcp --permanent

firewall-cmd --reload
```

Avec centos7, allez sous /etc/docker, si le fichier daemon.json n'existe pas, le créer sinon y mettre le contenu suivants sur master et node :

```
{
  "metrics-addr" : "0.0.0.0:9323",
  "experimental" : true
}
```

Source de la documentation principale : https://itnext.io/docker-swarm-monitoring-4dfe88c72d56

Commandes rapides sur master :

```
git clone https://github.com/skynt01/docker-swarm-monitor

docker stack deploy -c docker-compose.stack.yml docker-swarm-monitor
```

## Integrer à grafana

Ensuite il est possible d'integrer le endpoint de prometheus sur grafana.
