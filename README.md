# opsgenie-monit.sh

The scripts in this repository are wrappers for the Opsgenie API which will
open and close Opsgenie incidents from the command line. These scripts are
designed to help you integrate monit with Opsgenie.

This is based off of
* [pinterest/pagerduty\-monit: Wrapper scripts to integrate monit and PagerDuty\.](https://github.com/pinterest/pagerduty-monit)
* [etheriau/opsgenie\-monit](https://github.com/etheriau/opsgenie-monit)

Opsgenie API more info
* [Alert API](https://docs.opsgenie.com/docs/alert-api)

**Usage:** `opsgenie-trigger`

**Example:** `opsgenie-trigger` on the server "webserver1.example.com"
will trigger a Opsgenie alert
with the subject "${MONIT_SERVICE} ${MONIT_EVENT} on webserver1.example.com".

* ref. [ENVIRONMENT](https://mmonit.com/monit/documentation/monit.html#ENVIRONMENT)

While the script is written for monit specifically,
You can create alerts manually.

```
/etc/monit/opsgenie-trigger "Service" "Event"
```

**Prerequisites:** Requires [curl](https://curl.se/) command.

**To configure Opsgenie:** To configure OpsGenie:

* login as an administrator and goto the Teams tab
* Select your team and Integrations Tag
* Add `API`(Rest API HTTPS over JSON) Integration
* Be sure to set `API Key` to the `OPSGENIE_API_KEY` variable in both scripts.

**To configure Monit:** To configure Monit to trigger an event with this
script, save the script in a location like /etc/monit; make sure it's executable;
and use an action like this in your monitrc file:

```
    exec "/etc/monit/opsgenie-trigger"
```

Example monit stanza to trigger an incident if nginx is not present:

```
    check process nginx with pidfile /var/run/nginx.pid
        if does not exist for 3 cycles
            then exec "/etc/monit/opsgenie-trigger"
        else if passed for 3 cycles
            then exec "/etc/monit/opsgenie-resolve"
```
