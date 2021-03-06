---
title: Проверка клиента
ms.date: 03/30/2017
ms.assetid: f0c1f805-1a81-4d0d-a112-bf5e2e87a631
ms.openlocfilehash: 641d5e84c09575574ff6b06888d156c4b4aa0a38
ms.sourcegitcommit: 581ab03291e91983459e56e40ea8d97b5189227e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2019
ms.locfileid: "70040113"
---
# <a name="client-validation"></a>Проверка клиента
Службы часто публикуют метаданные, чтобы включить автоматическое создание и настройку типов прокси клиента. Если служба не является доверенной, клиентские приложения должны убедиться, что метаданные соответствуют политике клиентского приложения в плане безопасности, транзакций, типа контракта службы и т. д. В следующем образце показано, как создать поведение конечной точки клиента, которое проверяет конечную точку службы на предмет безопасности использования.  
  
 Служба предоставляет четыре конечных точки службы. Первая конечная точка использует WSDualHttpBinding, вторая - проверку подлинности NTLM, третья конечная точка включает поток транзакций, а четвертая использует проверку подлинности на основе сертификатов.  
  
 Для извлечения метаданных для службы клиент использует класс <xref:System.ServiceModel.Description.MetadataResolver>. Клиент реализует политику запрещения дуплексных привязок, проверки подлинности NTLM и потока транзакций с помощью поведения проверки. Для каждого <xref:System.ServiceModel.Description.ServiceEndpoint> экземпляра, импортированного из метаданных службы, клиентское приложение добавляет экземпляр `InternetClientValidatorBehavior` поведения <xref:System.ServiceModel.Description.ServiceEndpoint> конечной точки в перед попыткой использовать клиент Windows Communication Foundation (WCF) для подключения к Конечная точка. Метод `Validate` этого поведения выполняется до вызова каких-либо операций в службе и реализует политику клиента, создавая исключение `InvalidOperationExceptions`.  
  
### <a name="to-build-the-sample"></a>Сборка образца  
  
1. Чтобы выполнить сборку решения, следуйте инструкциям в разделе [Создание примеров Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
### <a name="to-run-the-sample-on-the-same-computer"></a>Запуск образца на одном компьютере  
  
1. Откройте Командная строка разработчика для Visual Studio с правами администратора и запустите программу Setup. bat из папки примеров установки. При этом устанавливаются все сертификаты, необходимые для выполнения образца.  
  
2. Запустите приложение службы из каталога \service\bin\Debug.  
  
3. Запустите клиентское приложение из каталога \client\bin\Debug. Действия клиента отображаются в консольном приложении клиента.  
  
4. Если клиент и служба не могут обмениваться данными, см. раздел [Советы по устранению неполадок для примеров WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
5. После завершения работы образца запустите файл Cleanup.bat, чтобы удалить сертификаты. В других образцах обеспечения безопасности используются те же сертификаты.  
  
### <a name="to-run-the-sample-across-computers"></a>Запуск образца на нескольких компьютерах  
  
1. На сервере в Командная строка разработчика для запуска Visual Studio с правами администратора введите `setup.bat service`. При запуске `setup.bat` с аргументомсоздаетсясертификатслужбысполнымдоменнымименемкомпьютераиэкспортируетсясертификатслужбывфайлсименемService.cer.`service`  
  
2. Измените App.config на сервере так, чтобы в файле отражалось новое имя сертификата. То есть измените `findValue` атрибут [ \<в элементе serviceCertificate >](../../../../docs/framework/configure-apps/file-schema/wcf/servicecertificate-of-clientcredentials-element.md) на полное доменное имя компьютера.  
  
3. Скопируйте файл Service.cer из каталога службы в клиентский каталог на клиентском компьютере.  
  
4. На клиенте откройте Командная строка разработчика для Visual Studio с правами администратора и введите `setup.bat client`. При запуске `setup.bat` с аргументомсоздаетсясертификатклиентасименемClient.comиэкспортируетсясертификатклиентавфайлсименемClient.cer.`client`  
  
5. В файле client.cs измените значение адреса конечной точки MEX и `findValue` для задания сертификата сервера по умолчанию таким образом, чтобы они соответствовали новому адресу службы. Для этого замените имя localhost полным именем домена сервера. Выполните перестроение.  
  
6. Скопируйте файл Client.cer из клиентского каталога в каталог службы на сервере.  
  
7. На клиенте запустите Импортсервицецерт. bat в Командная строка разработчика для Visual Studio, открытой с правами администратора. Он импортирует сертификат службы из файла Service.cer в хранилище CurrentUser - TrustedPeople.  
  
8. На сервере запустите Импортклиентцерт. bat в Командная строка разработчика для Visual Studio, открытой с правами администратора. При этом импортируется сертификат клиента из файла Client.cer в хранилище «LocalMachine - TrustedPeople».  
  
9. На компьютере службы постройте проект службы в Visual Studio и запустите файл service.exe.  
  
10. На клиентском компьютере запустите файл client.exe.  
  
    1. Если клиент и служба не могут обмениваться данными, см. раздел [Советы по устранению неполадок для примеров WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-clean-up-after-the-sample"></a>Очистка после образца  
  
- После завершения работы примера запустите в папке примеров файл Cleanup.bat.  
  
    > [!NOTE]
    > Этот скрипт не удаляет сертификаты службы на клиенте при запуске образца на нескольких компьютерах. Если вы выполнили примеры WCF, использующие сертификаты на нескольких компьютерах, обязательно очистите сертификаты службы, установленные в хранилище CurrentUser-TrustedPeople. Для этого используйте следующую команду: `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>. For example: certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com`.  
  
## <a name="see-also"></a>См. также

- [Использование метаданных](../../../../docs/framework/wcf/feature-details/using-metadata.md)
