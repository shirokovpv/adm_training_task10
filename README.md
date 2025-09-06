# adm_training_task10
<h1 align="center">Занятие 10. Практика c SELinux</h1>
<h3 class="western"><a name="_heading=h.h6i87lkp3f19"></a> <span style="font-family: Roboto, serif;"><span style="font-size: small;">Описание домашнего задания</span></span></h3>
<ol>
<li><span style="font-weight: 400;"> Запустить Nginx на нестандартном порту 3-мя разными способами:</span></li>
</ol>
<ul>
<li style="font-weight: 400;"><span style="font-weight: 400;">переключатели setsebool;</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">добавление нестандартного порта в имеющийся тип;</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">формирование и установка модуля SELinux.</span></li>
</ul>
<p><span style="font-weight: 400;">К сдаче:</span></p>
<ul>
<li style="font-weight: 400;"><span style="font-weight: 400;">README с описанием каждого решения (скриншоты и демонстрация приветствуются).&nbsp;</span></li>
</ul>
<p>&nbsp;</p>
<ol start="2">
<li><span style="font-weight: 400;"> Обеспечить работоспособность приложения при включенном selinux.</span></li>
</ol>
<ul>
<li style="font-weight: 400;"><span style="font-weight: 400;">развернуть приложенный стенд </span><a href="https://github.com/Nickmob/vagrant_selinux_dns_problems"><span style="font-weight: 400;">https://github.com/Nickmob/vagrant_selinux_dns_problems</span></a><span style="font-weight: 400;">;&nbsp;</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">выяснить причину неработоспособности механизма обновления зоны (см. README);</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">предложить решение (или решения) для данной проблемы;</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">выбрать одно из решений для реализации, предварительно обосновав выбор;</span></li>
<li style="font-weight: 400;"><span style="font-weight: 400;">реализовать выбранное решение и продемонстрировать его работоспособность.</span></li>
</ul>
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<h3 class="western"><a name="_heading=h.df570rpzx1qg"></a><span style="font-family: Roboto, serif;"><span style="font-size: small;">Используемые ОС</span></span></h3>
<p style="line-height: 108%; margin-bottom: 0.28cm;" align="justify"><span style="font-family: Roboto, serif;">Хостовая ОС Ubuntu 24.04 Desktop. Гостевая ОС Almalinux/9. VirtualBox версия 7.0.16_Ubuntu r162802</span></span></p>
<p style="line-height: 100%; margin-bottom: 0cm;">&nbsp;</p>
<h3 class="western"><span style="font-family: Roboto, serif;"><span style="font-size: small;">Выполнение</span></span></h3>
<p>&nbsp;</p>
<ul>
<li><strong>Создаём виртуальную машину</strong></li>
</ul>
<p><span style="font-weight: 400;">Код стенда получен из репозитория: </span><a href="https://github.com/Nickmob/vagrant_selinux"><span style="font-weight: 400;">https://github.com/Nickmob/vagrant_selinux</span></a><span style="font-weight: 400;">&nbsp;</span></p>
<p>Ввиду невозможности в нынешних реалиях воспользоваться стандартными репозиториями Vagrant (в РФ они сейчас не доступны), предложено обходное решение. Использован репозиторий, развернутый на <a href="https://vagrant.elab.pro/">https://vagrant.elab.pro/</a></p>
<p>Так как официальный пакет последней версии Vagrant также не доступен для скачивания, пакет взят оттуда же. Версия 2.3.5.</p>
<img width="531" height="116" alt="image" src="https://github.com/user-attachments/assets/87504f0e-594b-4c8d-98d3-03af890bb85c" />
<p>&nbsp;</p>
<p>Для подключения репозитория надо добавить в Vagrant-файл строку:</p>
<p><code>ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'</code></p>
<p>И изменить box_version на 1.0.0 (так в репозитории, если туда зайти, см. скрин). Измененный Vagrant-файл прикладываю сюда.</p>
<img width="418" height="348" alt="image" src="https://github.com/user-attachments/assets/0841f873-93b7-401c-bc62-84df24bc5541" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Результатом выполнения команды </span><em><span style="font-weight: 400;">vagrant up </span></em><span style="font-weight: 400;">станет созданная виртуальная машина с установленным nginx, который работает на порту TCP 4881. Порт TCP 4881 уже проброшен до хоста. SELinux включен.</span></p>
<p><span style="font-weight: 400;">Во время развёртывания стенда попытка запустить nginx завершится с ошибкой:</span></p>
<img width="831" height="534" alt="image" src="https://github.com/user-attachments/assets/f03bb5ef-d229-419c-bc26-5ff5b8df8316" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Данная ошибка появляется из-за того, что SELinux блокирует работу nginx на нестандартном порту.</span></p>
<p><span style="font-weight: 400;">Заходим на ВМ: </span><em><span style="font-weight: 400;">vagrant ssh</span></em></p>
<p><span style="font-weight: 400;">Дальнейшие действия выполняются от пользователя root.</span></p>
<p><span style="font-weight: 400;">Переходим в root-пользователя: sudo -i</span></p>
<img width="435" height="71" alt="image" src="https://github.com/user-attachments/assets/f7b48211-8a76-4074-895a-293b24ccb629" />
<p>&nbsp;</p>
<ul>
<li><strong>Запуск nginx на нестандартном порту 3-мя разными способами&nbsp;</strong></li>
</ul>
<p><span style="font-weight: 400;">Для начала проверим, что в ОС отключен файервол: </span><em><span style="font-weight: 400;">systemctl status firewalld</span></em></p>
<img width="818" height="139" alt="image" src="https://github.com/user-attachments/assets/f071764b-eac3-4ded-81aa-69e007114696" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Также можно проверить, что конфигурация nginx настроена без ошибок: </span><em><span style="font-weight: 400;">nginx -t</span></em></p>
<img width="683" height="96" alt="image" src="https://github.com/user-attachments/assets/d0470c48-c718-431d-910c-983528e1dd90" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Далее проверим режим работы SELinux: </span><span style="font-weight: 400;">getenforce </span></p>
<img width="338" height="70" alt="image" src="https://github.com/user-attachments/assets/1219a135-5650-485f-9a46-282ca0a0e351" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Данный режим означает, что SELinux будет блокировать запрещенную активность.</span></p>
<h3><strong>Разрешим в SELinux работу nginx на порту TCP 4881 c помощью переключателей setsebool</strong></h3>
<p><span style="font-weight: 400;">Находим в логах (/var/log/audit/audit.log) информацию о блокировании порта</span></p>
<img width="807" height="113" alt="image" src="https://github.com/user-attachments/assets/7f5feaff-cceb-4a1d-b0d7-d3f2a5aebc84" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">1757152388.186:709</span></p>
<p><span style="font-weight: 400;">Копируем время, в которое был записан этот лог, и с помощью утилиты audit2why смотрим </span> <em><span style="font-weight: 400;">grep 1757152388.186:709 /var/log/audit/audit.log | audit2why</span></em></p>
<img width="807" height="290" alt="image" src="https://github.com/user-attachments/assets/66eb8226-da72-48a3-9dab-e2f9bb5cbe18" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Утилита audit2why покажет, почему трафик блокируется. Исходя из вывода утилиты, мы видим, что нам нужно поменять параметр nis_enabled.</span></p>
<p><span style="font-weight: 400;">Включим параметр nis_enabled и перезапустим nginx:</span></p>
<p><em><span style="font-weight: 400;">setsebool </span></em><strong><em>-</em></strong><em><span style="font-weight: 400;">P nis_enabled on</span></em></p>
<p><em><span style="font-weight: 400;">systemctl restart nginx</span></em></p>
<p><em><span style="font-weight: 400;">systemctl status nginx</span></em></p>
<img width="807" height="508" alt="image" src="https://github.com/user-attachments/assets/086c1416-a803-4095-9888-d00715d642ef" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Как видим, nginx работает. Проверить статус параметра можно с помощью команды: </span><em><span style="font-weight: 400;">getsebool -a | grep nis_enabled</span></em></p>
<img width="523" height="73" alt="image" src="https://github.com/user-attachments/assets/29603894-ad0a-41f1-b16f-dfd5723f27fd" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Вернём запрет работы nginx на порту 4881 обратно. Для этого отключим nis_enabled: </span><em><span style="font-weight: 400;">setsebool -P nis_enabled off</span></em></p>
<p><span style="font-weight: 400;">После отключения nis_enabled служба nginx (когда мы ее перезапустим) снова не запустится.</span></p>
<img width="809" height="491" alt="image" src="https://github.com/user-attachments/assets/bd59d07c-26ae-402e-b828-078879a30486" />
<p>&nbsp;</p>
<h3><strong>Теперь разрешим в SELinux работу nginx на порту TCP 4881 c помощью добавления нестандартного порта в имеющийся тип:</strong></h3>
<p><span style="font-weight: 400;">Поиск имеющегося типа, для http трафика: </span><em><span style="font-weight: 400;">semanage port -l | grep http</span></em></p>
<img width="809" height="166" alt="image" src="https://github.com/user-attachments/assets/a00faabc-659e-4ffd-9cac-0f257c9d6662" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Добавим порт в тип http_port_t: </span><em><span style="font-weight: 400;">semanage port -a -t http_port_t -p tcp 4881</span></em></p>
<img width="809" height="139" alt="image" src="https://github.com/user-attachments/assets/1619c758-3cac-4d38-97ab-16ec1cebb8e4" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Теперь перезапускаем службу nginx и проверим её работу: </span><em><span style="font-weight: 400;">systemctl restart nginx</span></em></p>
<img width="809" height="483" alt="image" src="https://github.com/user-attachments/assets/d779bbc9-82dd-4929-a9b5-9218e8477936" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Как видим, nginx работает.</span></em></p>
<p><span style="font-weight: 400;">Удалить нестандартный порт из имеющегося типа можно с помощью команды: </span><em><span style="font-weight: 400;">semanage port -d -t http_port_t -p tcp 4881</span></em></p>
<img width="809" height="484" alt="image" src="https://github.com/user-attachments/assets/b9e3c3f6-ce26-483f-95e4-80910dd2d046" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">После этого nginx снова не работает.</span></em></p>
<h3><strong>Разрешим в SELinux работу nginx на порту TCP 4881 c помощью формирования и установки модуля SELinux:</strong></h3>
<p><span style="font-weight: 400;">Сейчас nginx не работает (вернули все блокировки обратно). Посмотрим логи SELinux, которые относятся к Nginx: grep nginx /var/log/audit/audit.log</span></p>
<p><strong>...</strong></p>
<p><strong>type=SERVICE_START msg=audit(1757160538.600:873): pid=1 uid=0 auid=4294967295 ses=4294967295 subj=system_u:system_r:init_t:s0 msg='unit=nginx comm="systemd" exe="/usr/lib/systemd/systemd" hostname=? addr=? terminal=? res=success'UID="root" AUID="unset"<br />type=SERVICE_STOP msg=audit(1757160757.852:876): pid=1 uid=0 auid=4294967295 ses=4294967295 subj=system_u:system_r:init_t:s0 msg='unit=nginx comm="systemd" exe="/usr/lib/systemd/systemd" hostname=? addr=? terminal=? res=success'UID="root" AUID="unset"<br />type=AVC msg=audit(1757160757.908:877): avc: denied { name_bind } for pid=10012 comm="nginx" src=4881 scontext=system_u:system_r:httpd_t:s0 tcontext=system_u:object_r:unreserved_port_t:s0 tclass=tcp_socket permissive=0<br />type=SYSCALL msg=audit(1757160757.908:877): arch=c000003e syscall=49 success=no exit=-13 a0=6 a1=55eb53baaff0 a2=10 a3=7ffd5c1bded0 items=0 ppid=1 pid=10012 auid=4294967295 uid=0 gid=0 euid=0 suid=0 fsuid=0 egid=0 sgid=0 fsgid=0 tty=(none) ses=4294967295 comm="nginx" exe="/usr/sbin/nginx" subj=system_u:system_r:httpd_t:s0 key=(null)ARCH=x86_64 SYSCALL=bind AUID="unset" UID="root" GID="root" EUID="root" SUID="root" FSUID="root" EGID="root" SGID="root" FSGID="root"<br />type=SERVICE_START msg=audit(1757160757.916:878): pid=1 uid=0 auid=4294967295 ses=4294967295 subj=system_u:system_r:init_t:s0 msg='unit=nginx comm="systemd" exe="/usr/lib/systemd/systemd" hostname=? addr=? terminal=? res=failed'UID="root" AUID="unset"<br />[root@selinux ~]# <br /></strong></p>
<p><span style="font-weight: 400;">Воспользуемся утилитой audit2allow для того, чтобы на основе логов SELinux сделать модуль, разрешающий работу nginx на нестандартном порту:&nbsp;</span></p>
<p><em><span style="font-weight: 400;">grep nginx /var/log/audit/audit.log | audit2allow -M nginx</span></em></p>
<p>******************** IMPORTANT ***********************<br />To make this policy package active, execute:</p>
<p>semodule -i nginx.pp</p>
<p>[root@selinux ~]# </p>
<p><span style="font-weight: 400;">Audit2allow сформировал модуль, и сообщил нам команду, с помощью которой можно применить данный модуль: </span><em><span style="font-weight: 400;">semodule </span></em><strong><em>-</em></strong><em><span style="font-weight: 400;">i nginx.pp</span></em></p>
<img width="798" height="511" alt="image" src="https://github.com/user-attachments/assets/26ad6f44-924d-45c8-be0c-f9ea0c909e75" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Как видим, nginx работает.</span></em></p>
<p><span style="font-weight: 400;">При использовании модуля изменения сохранятся после перезагрузки.&nbsp;</span></p>
<p><span style="font-weight: 400;">Просмотр всех установленных модулей: </span><em><span style="font-weight: 400;">semodule -l</span></em></p>
<p><span style="font-weight: 400;">Для удаления модуля воспользуемся командой: </span><em><span style="font-weight: 400;">semodule -r nginx</span></em></p>
<img width="798" height="536" alt="image" src="https://github.com/user-attachments/assets/f20b6071-9c3d-4006-802a-e321efe5ade3" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">После удаления модуля nginx снова не работает.</span></em></p>
<p>&nbsp;</p>
<ul>
<li><strong>Обеспечение работоспособности приложения при включенном SELinux</strong></li>
</ul>
<p>&nbsp;</p>






