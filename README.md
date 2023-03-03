# test2
<pre>
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from email.mime.application import MIMEApplication

# Set up the SMTP server
smtp_server = 'smtp.gmail.com'
smtp_port = 587
smtp_username = 'kennytang613@gmail.com'
smtp_password =  'tpfamsrsbsnfwsjr'
smtp_tls = True

# Set up the email message
sender_email = smtp_username
receiver_email = smtp_username
subject = 'Test Email with Attachment'
body = 'This is a test email sent from Python with an attachment.'
message = MIMEMultipart('alternative')
message['Subject'] = subject
message['From'] = sender_email
message['To'] = receiver_email

# Attach the body of the email
text = MIMEText(body, 'plain')
message.attach(text)

# Attach the file to the email
#filename = ['1.parquet','2.parquet','3.parquet','4.parquet','5.parquet']
filename = ['python code.txt','Python Regular Expression.txt']
with open(i, 'rb') as attachment:
    file = MIMEApplication(attachment.read(), _subtype='txt')
    file.add_header('Content-Disposition', 'attachment', filename=i)
    message.attach(file)

# Log in to the SMTP server and send the email
with smtplib.SMTP(smtp_server, smtp_port) as server:
    server.ehlo()
    if smtp_tls:
        server.starttls()
    server.login(smtp_username, smtp_password)
    server.sendmail(sender_email, receiver_email, message.as_string())
</pre>
