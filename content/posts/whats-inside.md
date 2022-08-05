+++
date = 2021-09-14T15:31:01Z
description = ""
draft = false
slug = "whats-inside"
title = "what's inside"
categories = ["meta"]
+++


domain for my homelab

current project :  using terraform to generate a kube cluster (1+2 nodes), using Proxmox API and migrate the following docker-compose stack to it.

actually docker-compose but slowly migrating to kube

medias :emby (self-hosted netflix), sonarr (tv series indexer), radarr (movies indexer), bazarr (subtitles indexer), prowlarr (indexer indexer), navidrome (self-hosted winamp revival), qbitorrent (speaks for itself), ghost (blog platform)

system & network :diun (docker images management), gitea (self-hosted github), openvpn (self-hosted VPN), pihole (LAN-wide adblocker), traefik (HTTPS ingress for the whole platform)

metrics & monitoring :cadvisor (docker metrics), fbx_telegraf (freebox delta metrics), influxdb (time-series db for metrics), node_exporter (metrics from the host), snmp_exporter (metrics from SNMP devices), pihole_exporter (metrics from pihole), prometheus (aggregates above metrics), grafana (well, graphs & alerting)

hardware & misc :Proxmox 6 hosting a debian 9 VM only for docker (16G RAM) + Harbor VM (proxy to docker hub)

