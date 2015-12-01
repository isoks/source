title: 'c#发送激活邮件'
date: 2015-09-22 18:58:29
tags: .Net
---

**c#发送注册激活邮件. **

```
	public static void Sends(string emails, string activeCode,HttpContext context)
        {
            string formto = "7348@qq.com";
            string to = emails;   //接收邮箱
            string content = "激活页面,请勿关闭！";
            string body = "点击下面链接激活账号，48小时生效，否则重新注册账号，链接只能使用一次，请尽快激活！</br><a href=http://" + context.Request.Url.Authority.ToString() + "/ashx/LoginHandler.ashx?type=email&activecode=" + activeCode + "&emails=" + emails + ">点击激活</a>";
            string name = "7348@qq.com";
            string upass = "密码";
            string smtp = "smtp.qq.com";
            SmtpClient _smtpClient = new SmtpClient();
            _smtpClient.DeliveryMethod = SmtpDeliveryMethod.Network;//指定电子邮件发送方式
            _smtpClient.Host = smtp; //指定SMTP服务器
            _smtpClient.Credentials = new System.Net.NetworkCredential(name, upass);//用户名和密码
            MailMessage _mailMessage = new MailMessage();
            //发件人，发件人名
            _mailMessage.From = new MailAddress(formto, "XXX");
            //收件人
            _mailMessage.To.Add(to);
            _mailMessage.SubjectEncoding = System.Text.Encoding.GetEncoding("gb2312");
            _mailMessage.Subject = content;//主题

            _mailMessage.Body = body;//内容
            _mailMessage.BodyEncoding = System.Text.Encoding.GetEncoding("gb2312");//正文编码
            _mailMessage.IsBodyHtml = true;//设置为HTML格式
            _mailMessage.Priority = MailPriority.High;//优先级  
            try
            {
                _smtpClient.Send(_mailMessage);
            }
            catch (Exception)
            {

            }

        }
```