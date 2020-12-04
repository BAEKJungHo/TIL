# JavaMailSender

## MimeMessagePreparator 를 이용한 대량 메일 보내기

```java
public void sendMail(List<MailVO> mails) {
	LOGGER.info("started send email (log..)");
	MimeMessagePreparator[] mimeMessagePreparators = new MimeMessagePreparator[mails.size()];
	int i=0;
	for(final MailVO mailVO : mails) {
		mimeMessagePreparators[i++] = new MimeMessagePreparator() {
			@Override
			public void prepare(MimeMessage mimeMessage) throws Exception {
				final MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, true, "UTF-8");
				setMailContents(helper, mailVO);
				setAttachedFile(helper, mailVO);
			}
		};
	}
	mailSender.send(mimeMessagePreparators);
	LOGGER.info("ended send email (log..)");
}
```

## References.

> https://offbyone.tistory.com/167
