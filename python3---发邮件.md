# python3---发邮件.

```
import smtplib
from email.header import Header
from email.mime.text import MIMEText


content = get_email_content()
message = MIMEText(content, "html", MAIL_ENCODING)
message["From"] = Header("weekly-bot", MAIL_ENCODING)
message["To"] = Header("Reader")
message["Subject"] = Header("weekly", MAIL_ENCODING)
try:
    smtp_obj = smtplib.SMTP_SSL(MAIL_HOST)
    smtp_obj.login(MAIL_USER, MAIL_PASS)
    smtp_obj.sendmail(MAIL_SENDER, MAIL_RECEIVER, message.as_string())
    smtp_obj.quit()
except Exception as e:
    print(e)
```



## 参考
- [core.py](https://github.com/chenjiandongx/weekly-email-subscribe/blob/master/core.py)
