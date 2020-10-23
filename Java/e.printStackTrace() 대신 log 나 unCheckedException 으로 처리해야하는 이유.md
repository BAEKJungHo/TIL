# e.printStackTrace() 대신 log 나 unCheckedException 으로 처리해야하는 이유

e.printStackTrace() 는 시스템 출력이기 때문에 서버에 로그를 남기기 위해서는 log 나 unCheckedException 으로 처리해야한다.
