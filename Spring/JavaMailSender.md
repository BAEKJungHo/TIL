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

private void setMailContents(MimeMessageHelper msgHelper, MailVO mailVO) throws Exception {
	msgHelper.setFrom(mailVO.getFrom());
	msgHelper.setTo(mailVO.getTo());
	msgHelper.setSubject(mailVO.getTitle());
	msgHelper.setText(mailVO.getContent(), true);
	msgHelper.setSentDate(new Date());
}

private void setAttachedFile(MimeMessageHelper msgHelper, MailVO mailVO) throws Exception {
	List<MultipartFile> attFileList = mailVO.getAttFileList();
	if (attFileList == null) {
		return;
	}
	for (int i=0; i < attFileList.size(); i++) {
		MultipartFile file = attFileList.get(i);
		if (file.getSize() > 0) {
			msgHelper.addAttachment(file.getOriginalFilename(), new ByteArrayResource(file.getBytes()));
		}
	}
}
```

## References.

> https://offbyone.tistory.com/167
