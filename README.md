# icinga2-slack-notification

1. Tạo 1 incoming-webhook trên slack.

- Copy Webhook URL và paste vào file *slack-service-notification.sh* và *slack-host-notification.sh*

- Chỉnh sửa SLACK\_CHANNEL và SLACK\_BOTNAME 

2. Add *slack-service-notification.sh* to */etc/icinga2/scripts* directory

3. Add the contents of *slack-service-notification.conf* to your *templates.conf* (or keep it separate depending on your setup)

4. Add the contents of *slack-service-notification-command.conf* to your *commands.conf* (or keep it separate depending on your setup)

5. Add an entry to your *notifications.conf* (see example below)
```
apply Notification "slack" to Service {
  import "slack-service-notification"
  user_groups = [ "oncall" ]
  interval = 30m
  assign where host.vars.sla == "24x7"
}
```
6. Set up a new user and usergroup
```
object User "oncall_alerts" {
  import "generic-user"

  display_name = "oncall alerts"
  groups = [ "oncall" ]
  states = [ OK, Warning, Critical ]
  types = [ Problem, Recovery ]

  email = "my@email.com"
}

object UserGroup "oncall" {
  display_name = "oncall"
}
```
