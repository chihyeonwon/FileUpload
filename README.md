## 파일 업로드 공격
이 도구를 이용하여 허용받지 않은 서비스 대상으로 해킹을 시도하는 행위는 범죄 행위 입니다.        
해킹을 시도할 때에 발생하는 법적인 책임은 그것을 행한 사용자에게 있다는 것을 명심하시기 바랍니다.      
File Upload 취약점은 파일을 첨부할 수 있는 게시판 등에서 악성 스크립트 파일을 웹 서버에 업로드하여 웹쉘 등을 실행 시킬수 있는 취약점입니다.      

![image](https://github.com/user-attachments/assets/2afe7927-9d9f-4a07-b967-bce82c74cbf3)
해당화면은 File Upload 취약점 진단항목의 화면이며, 간단하게 파일을 업로드 할수 있게 화면이 이루어져 있습니다.    
해당 진단항목 역시 링크를 타고 들어 가시면 File Upload 취약점에 대한 정보를 보실수 있습니다.    

![image](https://github.com/user-attachments/assets/e18e8855-3fbb-42a8-8251-79e614d8166f)
Low레벨의 소스코드를 보시면 업로드 파일에 대한 검증없이 모든 파일에 대하여 업로드를 허용하고 있습니다.    
그럼 간단하게 파일을 업로드 해보도록 하겠습니다.      

![image](https://github.com/user-attachments/assets/61301323-fc6f-4496-bb78-1469d5aeafe6)
Low 레벨은 소스코드상의 아무런 검증 로직이 존재하지 않으므로, 파일을 선택후 [Upload] 버튼을 클릭하면 업로드 성공 메시지를 보실 수 있습니다.       
그럼 실제 디렉토리를 찾아가 파일이 업로드 되었는지 확인해 보도록 하겠습니다.       

![image](https://github.com/user-attachments/assets/cd17b3ab-31ee-4c1e-be6f-f6b35b601cac)
Low 레벨은 소스코드상의 아무런 검증 로직이 존재하지 않으므로, 파일을 선택후 [Upload] 버튼을 클릭하면 업로드 성공 메시지를 보실 수 있습니다.    
그럼 실제 디렉토리를 찾아가 파일이 업로드 되었는지 확인해 보도록 하겠습니다.      

![image](https://github.com/user-attachments/assets/02f50390-c1ba-4004-be41-f297dcffaf1a)
저는 예전에 만들어 놓았던 CSRFTEST.html 파일을 업로드 하였습니다. 해당 디렉토리를 가보면 정상적으로 업로드가 되어 있는것을 확인 할 수 있습니다.      

![image](https://github.com/user-attachments/assets/9d955d6c-3457-4c3f-8e18-a474145f635d)

업로드 한 파일을 실행했을때의 화면 입니다. 만약 해당 파일이 스크립트로 짜여진 파일이라면 원격코드 및 임의코드를 실행시켜 서버에 악의적인 행위들을 할수 있습니다.

![image](https://github.com/user-attachments/assets/018da8ab-77a0-4ad4-a140-86d845b0ab18)

Medium레벨의 소스 코드 입니다. 해당 소스코드를 살펴보면 업로드 파일 타입이 image/jpeg, image/png 이고 크기가 100,000 이하인 파일만 업로드 할 수 있게 되어 있습니다.       
해당 코드는 파일 업로드시 이미지 타입을 변환하면 간단히 우회가 가능합니다.      
Medium레벨 테스트를 하기 전에 DVWA Security -> Security Level -> Medium 으로 바꿔 주시고 조금 전 올렸던 파일은 삭제를 해주시기 바랍니다.    
그럼 Medium 레벨로 바꾼 후 동일한 파일을 업로드 해보도록 하겠습니다.    

![image](https://github.com/user-attachments/assets/d4bef276-89be-4646-89ce-b1c78aea4d50)
해당 업로드할 파일의 확장자가 html 파일이기 때문에 파입타입 형식이 맞지 않아 업로드가 되지 않는 다는 메시지를 표시하고 있습니다.    
그럼 버프스위트를 이용하여 이러한 이미지 검증을 우회해보도록 하겠습니다.   

![image](https://github.com/user-attachments/assets/6fca5c18-930e-4c6f-b6c5-b544aa6f99ef)
업로드할 파일을 선택한후 [Upload] 버튼을 눌렀을때 버프스위트에서 패킷을 잡은 화면 입니다.

![image](https://github.com/user-attachments/assets/04805193-9618-4d8c-9fc3-e67847589cb0)
패킷을 잡은후 'Content-Type' 부분을 소스코드에서 허용되었던 파일 타입으로 수정후 (image/jpeg, image/png) [Forward] 버튼을 클릭하여 웹브라우저로 프록시를 넘겨줍니다.

![image](https://github.com/user-attachments/assets/63bc7b59-4cd5-4308-bd8d-862ac89401ef)
File Upload 검증을 우회하여 업로드에 성공하였습니다. 실제 파일 디렉토리를 확인하신 후 업로드가 되었는지 확인하시기 바랍니다.     
High 레벨의 소스코드는 확장자만 검증하기 때문에 확장자 검증으로 우회가 가능합니다. 지금까지의 내용을 종합하여 확장자 검증 우회를 직접 해보시기 바랍니다.    

    

## More Information
https://www.owasp.org/index.php/Unrestricted_File_Upload
https://blogs.securiteam.com/index.php/archives/1268
https://www.acunetix.com/websitesecurity/upload-forms-threat/

