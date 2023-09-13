# prometheus-skype-alerts
Send Prometheus alerts via Skype

Usage
-----
# 
Must support multiple users in config
Edit the configuration file (defaults to ``/etc/prometheus/skype-alerts.yml``):
```yaml
skype_user: 'alerts@example.com'
password: 'PASSWORD'
to_user:
  - 'skype_username_to_send' # or LiveID 'live:.cid.cd13cb' or ChatID '19:XXXX@thread.skype'
  - 'more_usernames_to_send'
listen_address: '0.0.0.0'
listen_port: 9478
format: 'short'
```

Configure the webhook in alertmanager:
```yaml
receivers:
- name: 'skype-pager'
  webhook_configs:
  - url: 'http://192.168.2.1:9478/alert'
```

And run the web hook::

```shell
$ python3 prometheus-skype-alerts
```

Docker image
-------
You can use Dockerfile supplied with the package or run it from ghcr.io.
Provide configuration in ``/config.yaml``:
```shell
$ docker run -d -p 9478:9478 -v ./config.yaml:/config.yaml ghcr.io/yurcn/prometheus-skype-alerts
```

Testing
-------

The web hook can be accessed on three paths:

 * ``/alert``: used by Prometheus to deliver alerts, expects POST requests
   with JSON body
 * ``/test``: delivers a test message
 * ``/metrics``: exposes statistics about number of alerts received
