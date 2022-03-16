+++
title = 'Nextcloud aus der deutschen Cloud'
date = "2020-10-03T11:00:00"
image = 'https://res.cloudinary.com/dzw4emsdt/image/upload/v1601838025/selfscrum/git-automation-Platform_Implementation_zt5dnj.png'
author = 'Martin'
description = '"Good Enough Deployment" für kleine Projekte'
disableComments = true
categories = ['Betrieb', 'Community']
icon = 'fa-user-cog'
+++

## Motivation

.

Schon lange liegt bei mir Nextcloud als zu betrachtende Kommunikationslösung im Backlog. Immer wieder kam was dazwischen - nun habe ich einen echten Anwendungsfall. Der Git for Kids Learning Circle ist das erste Mal gestartet und hat außer zoom noch nicht so richtig eine Kommunikationsplattform. Was liegt da näher, als endlich einmal Nextcloud zu testen?

Angesichts der letzten Datenschutz-Rechtssprechung ist auch wieder klar geworden, dass _wir_ uns bewegen müssen, wir als Consumer der großen Plattformen. Solange ein profitabler Markt da ist, wird Rechtsprechung alleine keinen Wandel hervorrufen. Aus diesem Grund -und weil Community-Plattformen ja immer besonders viel mit persönlichen Daten zu tun haben- habe ich beschlossen, dieses Mal nicht in Richtung AWS zu schauen, sondern einen deutschen Cloud Provider zu nutzen. 

Der Tag der deutschen Einheit kommt wie gerufen für ein kleines Tagesprojekt.

## Vorhaben

Meine Wahl fällt auf die Hetzner Cloud, weil es dort günstige Angebote gibt (knapp 6 EUR gegenüber den vergleichbaren 30 EUR bei AWS) und vor allem, weil es dort einen Terraform Provider gibt, der mir die Plattform mit gewohnten Tools aufzubauen hilft. Rechenzentren liegen in Deutschland und Finnland, all dies auswählbar über die API.

Damit es schnell geht, werde ich diesmal nicht beim der tar-Distribution beginnen, sondern die Docker-Services von Nextcloud nutzen. Dort gibt es bereits fertig konfigurierte Beispiele mit Datenbank, Lets Encrypt-Verschlüsselung und Redis Cache. Für die Konfiguration werde ich Ansible einsetzen.

## Umsetzung

So wie im Bild soll die Zielarchitektur aussehen. Allerdings gibt es dafür ausgehend von [Nextcloud's Github Beispiel](https://github.com/nextcloud/docker/tree/master/.examples/docker-compose/with-nginx-proxy/mariadb-cron-redis/apache) noch einiges zu tun.

### Cloud Account

![nextcloud target architecture](https://res.cloudinary.com/dzw4emsdt/image/upload/v1601838025/selfscrum/git-automation-Platform_Implementation_zt5dnj.png)

Zunächst der Aufbau der virtuellen Maschine in der [Hetzner Cloud](https://accounts.hetzner.com). Einen Account braucht man dort, der ist kostenfrei und schnell angelegt. Zuerst ein API Token für Terraform:

![Hetzner API Token](https://res.cloudinary.com/dzw4emsdt/image/upload/v1601838524/selfscrum/he_token_utxjrj.png)

Den erzeugten Wert merken, der kommt nie wieder.

Dann auf der Linux-Console mit `ssh-keygen` einen ssh Key ohne Passwort erzeugt. Der Public Key kommt dann bei Hetzner in die Box. Den Private Key merken wir uns an geeigneter Stelle (zum Beispiel in der Datei `~/.ssh\id_rsa_hetzner`).

![Hetzner SSH Key](https://res.cloudinary.com/dzw4emsdt/image/upload/v1601838523/selfscrum/he_key_dvddig.png)

Mehr brauchen wir nicht. 

### Virtuelle Maschine

Das Terraform Script ist schnell geschrieben:

```
variable "access_token" {}
variable "env_name"  { }
variable "env_stage" { }
variable "system_function" {}
variable "instance_image" {}
variable "instance_type" {}
variable "location" { }
variable "keyname" {} 

provider "hcloud" {
  token = var.access_token
}

resource "hcloud_server" "server" {
  name        = format("%s-%s-%s", var.env_stage, var.env_name, var.system_function)
  image       = var.instance_image
  server_type = var.instance_type
  location    = var.location
  labels      = {
      "Name"     = var.env_name
      "Stage"    = var.env_stage
      "Function" = var.system_function
  }
  ssh_keys    = [ var.keyname ]
}

output "server-ip" {
    value = hcloud_server.server.ipv4_address
}
```

Da der Hetzner-Provider nicht zum Hashicorp-Standard gehört, braucht es noch eine Pointer-Datei `versions.tf` im selben Verzeichnis.

```
terraform {
  required_providers {
    hcloud = {
      source = "hetznercloud/hcloud"
    }
  }
  required_version = ">= 0.13"
}
```

Je nachdem, ob ihr Terraform auf der Kommandzeile oder in Terraform Cloud nutzt (letzteres ist zu empfehlen, da es sich schön mit dem git Repository verbindet), benötigt ihr noch ein Variablen-File oder die Variablen-Konfiguration in der Cloud. Ich nutze ein Terraform Script, dass mir den Terraform Cloud Workspace anlegt, so dass dies schnell getan ist.

`access_token`nimmt den Wert aus der Hetzner-Console auf, und `keyname` wird der Name, den wir dem SSH Key in der Hetzner-Console gegeben haben, in diesem Fall also `tfh-server`. Ich nehme als location Nürnberg (`nbg1`), als image `ubuntu-18.04` und für die Maschine wähle ich eine `cx21` aus der Hetzner-Liste - 2 VPCU, 4GB RAM und 40GB local Disk sollten erstmal für unser kleines Projekt reichen.

Terraform laufen lassen und siehe da - wir erhalten eine IP-Adresse, die wir mit dem erzeugten Private Key erreichen ( `ssh -i ~/.ssh/id_rsa_hetzner root@123.45.67.89` ). Durch ein paar Projektkonfigurationsdateien und einige Scripts in meiner Entwicklungsumgebung spare ich mir die vielen Parameter und rufe demnächst nur `ssh 123.45.67.89` auf, was ich noch ein paarmal tun muss.

### Ansible

Der Beispielcode von nextcloud will nämlich zunächst nach Ansible übertragen werden. Ich bleibe im Wesentlichen bei der Struktur des Beispiels. Nur an einigen Stellen hakt es noch, dort muss ich nachbessern.

Ich arbeite gerne mit den bei Ansible angegebenen StandardVerzeichnissen, so dass mein Playbook über einige Verzeichnisse verstreut ist. Ich spare mir hier die Erklärung, wie man ein playbook aufruft. Ich habe dazu ein Script gebaut, was nicht veröffentlichungsfähig ist :) Die Struktur und die folgenden Infos sollten aber für erfahrene ansible Benutzer ausreichend sein. Ich spare mir hier die Ausgabe des kompletten Codes und verweise auf eine [github-Kopie](https://github.com/selfscrum/nextcloud_server) meines ursprünglich in Gitlab angelegten Projekts.

![ansible_tree](https://res.cloudinary.com/dzw4emsdt/image/upload/v1601839735/selfscrum/nc_ansible_zjs3pf.png)

#### Hauptscript

Das Hauptscript `nextcloud_server.yml` startet nur die Rolle, in der die komplette Initialisierung stattfindet. Wichtig ist hier der python3 Verweis, da die Docker Tools ziemlich sensibel diesbezüglich sind.

```
- hosts: NEXTCLOUD_SERVER
  gather_facts: no
  ignore_errors: yes
  become: yes
  become_user: root
  vars:
    - ansible_python_interpreter: /usr/bin/python3
  roles:
    - 001_init_nextcloud_server
```

#### Initialisierungs-Script

Das `001_init_nextcloud_server/tasks/main.yml`Script enthält die komplette Konfiguration.
Wir installieren einige Hilfstools, sowie `docker-ce` und `docker-compose` und kopieren dann die für `docker-compose` benötigte Struktur.

Ein paar Daten sind variabel, so dass die ansible Jinja Templates zum Einsatz kommen. Das `docker-compose.yml.j2` Template zeigt gut die Komponenten und ihre Abhängigkeiten:

* `db` - Die Datenbank bleibt unverändert bis auf den Passwort-Parameter.
* `redis` - Musste ich neu bauen, um das Passwort in redis setzen zu können. Ohne Passwort, das bei redis und in der App Server Konfiguration identisch sein muss, fährt das System nicht hoch. Sonst ist es das Standard-redis Image.
* `app` - Unverändert bis auf die Variablendaten und das redis-Passwort in `REDIS_HOST_PASSWORD`. Die Modifikation der `config.php`-Datei habe ich mir hier gespart, um nicht auch diesen Container neu bauen zu müssen. Die Änderungen erfolgen im Anschluss an den Start, da die Docker-Volumes auch auf dem Host erreichbar sind. Nicht elegant aber pragmatisch.
* `cron` - Unverändert
* `proxy` - In der Definition unverändert, bis auf eine eingefügte Abhängigkeit. Da der Proxy grundsätzlich mit einem 503 - "Service nicht verfügbar"-Fehler antwortete, musste ich die nginx Definition ändern. Diese Definition erzeugt beim Start des Containers der nginx Server durch ein Template `nginx.tmpl`, was hier beim neuen Build mit eingepflegt wird.
* `letsencrypt-companion` - Unverändert, hier gab es keine Probleme.

Der letzte Aufruf im Initialisierungs-Script war eigentlich der Start von `docker-compose`. Der Server und der Web-Zugriff laufen damit bereits problemlos, wenn man von einigen Datenbank-Deadlocks beim Löschen von Dateien absieht, aber das scheint ein Standardproblem zu sein.

**Wichtig** ist, dass beim ersten Anmelden bereits die eingerichtete Domain zum Zugriff genutzt wird, weil das Nextcloud-Setup sich erst mit dem ersten Anmelden vervollständigt und dabei den Aufrufs-Kontext nutzt.

Allerdings musste ich noch eine Inkonsistenz beseitigen - Ich habe  zum Abschluss dieses Projekts wie oben schon beschrieben noch die `config.php` des App-Servers angepasst, da die externen Nextcloud-Clients nicht funktionierten. Grund ist, dass im Beispiel kein https-Protokoll definiert wurde, außerdem fehlen noch Einträge zu `overwriteprotocol` und `overwritehost`. Ein Restart des Containers sorgt für die Annahme dieser Änderungen, so dass nun auch Mobile Access möglich ist.

## Fazit

Mein Feiertagsprojekt ist für mich zufriedenstellend umgesetzt und unsere kleine Initiative wird gut damit arbeiten können. Für größere Produktionsumsetzungen müsste natürlich noch einiges getan werden, ebenso für Datensicherung, Systemhärtung usw. Aber hier reicht ein "Good Enough Deployment". 

Ein bisschen ärgerlich sind die Inkonsistenzen im Beispielcode, das hat viel Probieren und Ergründen von Fehlermeldungen benötigt, bis es lief. Ich bin froh. dass ich Terraform und Ansible nutze, so konnte ich immer wieder schnell neu aufsetzen, wenn ich etwas zerspielt hatte.

Mein erstes Terraform-Projekt außerhalb des üblichen AWS-Biotops hat gut funktioniert. Sehr erfreulich auch, dass die Kostenuhr bei Hetzner nach einem Tag Betrieb nur 25 Cents anzeigt...