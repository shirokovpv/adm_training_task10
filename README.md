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
<p><span style="font-weight: 400;">После удаления модуля nginx снова не работает. Первая часть задания выполнена. ВМ можно выключить, в дальнейшем понадобятся другие.</span></em></p>
<p>&nbsp;</p>
<ul>
<li><strong>Обеспечение работоспособности приложения при включенном SELinux</strong></li>
</ul>
<p><span style="font-weight: 400;">На хостовой машине установлены git, ansible и vagrant. Выполним клонирование репозитория:</span><span style="font-weight: 400;"><br /></span><em><span style="font-weight: 400;">git clone https://github.com/Nickmob/vagrant_selinux_dns_problems.git</span></em></p>
<img width="808" height="276" alt="image" src="https://github.com/user-attachments/assets/b9c82142-b51d-445d-890c-1d7298c9f384" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Перейдём в каталог со стендом: cd vagrant_selinux_dns_problems</span></em></p>
<p><span style="font-weight: 400;">Аналогично изменим Vagrant-файл (см. выше) для установки ВМ. </span></em></p>
<p><span style="font-weight: 400;">Установим ВМ с помощью команды: vagrant up</span></em></p>
<p><span style="font-weight: 400;">После того, как стенд развернется, проверим ВМ с помощью команды: </span><em><span style="font-weight: 400;">vagrant status</span></em></p>
<img width="711" height="232" alt="image" src="https://github.com/user-attachments/assets/25d5e5f9-838e-4e27-99a3-c75b41b0bb30" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Подключимся к клиенту: </span><em><span style="font-weight: 400;">vagrant ssh client</span></em></p>
<img width="711" height="537" alt="image" src="https://github.com/user-attachments/assets/d98c3199-9947-4eaa-be56-eb91a6cab376" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Попробуем внести изменения в зону: </span><em><span style="font-weight: 400;">nsupdate -k /etc/named.zonetransfer.key</span></em></p>
<p>[vagrant@client ~]$ nsupdate -k /etc/named.zonetransfer.key<br />&gt; server 192.168.50.10<br />&gt; zone ddns.lab<br />&gt; update add www.ddns.lab. 60 A 192.168.50.15<br />&gt; send<br />update failed: SERVFAIL<br />&gt; quit<br />[vagrant@client ~]$</p>
<p><span style="font-weight: 400;">Изменения внести не получилось. Давайте посмотрим логи SELinux, чтобы понять в чём может быть проблема.</span></p>
<p><span style="font-weight: 400;">Для этого воспользуемся утилитой audit2why:</span></p>
<img width="808" height="555" alt="image" src="https://github.com/user-attachments/assets/076c8668-bf41-4289-b495-37099524a340" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Подключимся к серверу ns01 и проверим логи SELinux:</span></p>
<img width="1848" height="735" alt="image" src="https://github.com/user-attachments/assets/96337b02-624f-4e4a-9a53-063288056aca" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">В логах мы видим, что ошибка в контексте безопасности. Целевой контекст </span><strong>named_conf_t</strong><span style="font-weight: 400;">.</span></p>
<p><span style="font-weight: 400;">Для сравнения посмотрим существующую зону (localhost) и её контекст:</span></p>
<p>[root@ns01 ~]# ls -alZ /var/named/named.localhost<br />-rw-r-----. 1 root named system_u:object_r:named_zone_t:s0 152 Jul 29 21:44 /var/named/named.localhost<br />[root@ns01 ~]# </p>
<p><span style="font-weight: 400;">У наших конфигов в </span><strong>/</strong><span style="font-weight: 400;">etc</span><strong>/</strong><span style="font-weight: 400;">named</span><span style="font-weight: 400;"> вместо типа </span><strong>named_zone_t</strong><span style="font-weight: 400;"> используется тип </span><strong>named_conf_t</strong><span style="font-weight: 400;">.</span></p>
<p><span style="font-weight: 400;">Проверим данную проблему в каталоге /etc/named:</span></p>
<img width="813" height="378" alt="image" src="https://github.com/user-attachments/assets/912859b4-b4ee-4d4a-88fb-3f0d51347fd8" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Тут мы также видим, что контекст безопасности неправильный. Проблема заключается в том, что конфигурационные файлы лежат в другом каталоге. Посмотреть в каком каталоги должны лежать, файлы, чтобы на них распространялись правильные политики SELinux можно с помощью команды: </span><em><span style="font-weight: 400;">sudo semanage fcontext -l </span></em><strong><em>|</em></strong><em><span style="font-weight: 400;"> grep named</span></em></p>
<p>[root@ns01 ~]# sudo semanage fcontext -l | grep named<br />/dev/gpmdata named pipe system_u:object_r:gpmctl_t:s0 <br />/dev/initctl named pipe system_u:object_r:initctl_t:s0 <br />/dev/xconsole named pipe system_u:object_r:xconsole_device_t:s0 <br />/dev/xen/tapctrl.* named pipe system_u:object_r:xenctl_t:s0 <br />/etc/named(/.*)? all files system_u:object_r:named_conf_t:s0 <br />/etc/named\.caching-nameserver\.conf regular file system_u:object_r:named_conf_t:s0 <br />/etc/named\.conf regular file system_u:object_r:named_conf_t:s0 <br />/etc/named\.rfc1912.zones regular file system_u:object_r:named_conf_t:s0 <br />/etc/named\.root\.hints regular file system_u:object_r:named_conf_t:s0 <br />/etc/rc\.d/init\.d/named regular file system_u:object_r:named_initrc_exec_t:s0 <br />/etc/rc\.d/init\.d/named-sdb regular file system_u:object_r:named_initrc_exec_t:s0 <br />/etc/rc\.d/init\.d/unbound regular file system_u:object_r:named_initrc_exec_t:s0 <br />/etc/rndc.* regular file system_u:object_r:named_conf_t:s0 <br />/etc/unbound(/.*)? all files system_u:object_r:named_conf_t:s0 <br />/usr/lib/systemd/system/named-sdb.* regular file system_u:object_r:named_unit_file_t:s0 <br />/usr/lib/systemd/system/named.* regular file system_u:object_r:named_unit_file_t:s0 <br />/usr/lib/systemd/system/unbound.* regular file system_u:object_r:named_unit_file_t:s0 <br />/usr/lib/systemd/systemd-hostnamed regular file system_u:object_r:systemd_hostnamed_exec_t:s0 <br />/usr/sbin/lwresd regular file system_u:object_r:named_exec_t:s0 <br />/usr/sbin/named regular file system_u:object_r:named_exec_t:s0 <br />/usr/sbin/named-checkconf regular file system_u:object_r:named_checkconf_exec_t:s0 <br />/usr/sbin/named-pkcs11 regular file system_u:object_r:named_exec_t:s0 <br />/usr/sbin/named-sdb regular file system_u:object_r:named_exec_t:s0 <br />/usr/sbin/unbound regular file system_u:object_r:named_exec_t:s0 <br />/usr/sbin/unbound-anchor regular file system_u:object_r:named_exec_t:s0 <br />/usr/sbin/unbound-checkconf regular file system_u:object_r:named_exec_t:s0 <br />/usr/sbin/unbound-control regular file system_u:object_r:named_exec_t:s0 <br />/usr/share/munin/plugins/named regular file system_u:object_r:services_munin_plugin_exec_t:s0 <br />/var/lib/softhsm(/.*)? all files system_u:object_r:named_cache_t:s0 <br />/var/lib/unbound(/.*)? all files system_u:object_r:named_cache_t:s0 <br />/var/log/named.* regular file system_u:object_r:named_log_t:s0 <br />/var/named(/.*)? all files system_u:object_r:named_zone_t:s0 <br />/var/named/chroot(/.*)? all files system_u:object_r:named_conf_t:s0 <br />/var/named/chroot/dev directory system_u:object_r:device_t:s0 <br />/var/named/chroot/dev/log socket system_u:object_r:devlog_t:s0 <br />/var/named/chroot/dev/null character device system_u:object_r:null_device_t:s0 <br />/var/named/chroot/dev/random character device system_u:object_r:random_device_t:s0 <br />/var/named/chroot/dev/urandom character device system_u:object_r:urandom_device_t:s0 <br />/var/named/chroot/dev/zero character device system_u:object_r:zero_device_t:s0 <br />/var/named/chroot/etc(/.*)? all files system_u:object_r:etc_t:s0 <br />/var/named/chroot/etc/localtime regular file system_u:object_r:locale_t:s0 <br />/var/named/chroot/etc/named\.caching-nameserver\.conf regular file system_u:object_r:named_conf_t:s0 <br />/var/named/chroot/etc/named\.conf regular file system_u:object_r:named_conf_t:s0 <br />/var/named/chroot/etc/named\.rfc1912.zones regular file system_u:object_r:named_conf_t:s0 <br />/var/named/chroot/etc/named\.root\.hints regular file system_u:object_r:named_conf_t:s0 <br />/var/named/chroot/etc/pki(/.*)? all files system_u:object_r:cert_t:s0 <br />/var/named/chroot/etc/rndc\.key regular file system_u:object_r:dnssec_t:s0 <br />/var/named/chroot/lib(/.*)? all files system_u:object_r:lib_t:s0 <br />/var/named/chroot/proc(/.*)? all files &lt;&lt;None&gt;&gt;<br />/var/named/chroot/run/named.* all files system_u:object_r:named_var_run_t:s0 <br />/var/named/chroot/usr/lib(/.*)? all files system_u:object_r:lib_t:s0 <br />/var/named/chroot/var/log directory system_u:object_r:var_log_t:s0 <br />/var/named/chroot/var/log/named.* regular file system_u:object_r:named_log_t:s0 <br />/var/named/chroot/var/named(/.*)? all files system_u:object_r:named_zone_t:s0 <br />/var/named/chroot/var/named/data(/.*)? all files system_u:object_r:named_cache_t:s0 <br />/var/named/chroot/var/named/dynamic(/.*)? all files system_u:object_r:named_cache_t:s0 <br />/var/named/chroot/var/named/named\.ca regular file system_u:object_r:named_conf_t:s0 <br />/var/named/chroot/var/named/slaves(/.*)? all files system_u:object_r:named_cache_t:s0 <br />/var/named/chroot/var/run/dbus(/.*)? all files system_u:object_r:system_dbusd_var_run_t:s0 <br />/var/named/chroot/var/run/named.* all files system_u:object_r:named_var_run_t:s0 <br />/var/named/chroot/var/tmp(/.*)? all files system_u:object_r:named_cache_t:s0 <br />/var/named/chroot_sdb/dev directory system_u:object_r:device_t:s0 <br />/var/named/chroot_sdb/dev/null character device system_u:object_r:null_device_t:s0 <br />/var/named/chroot_sdb/dev/random character device system_u:object_r:random_device_t:s0 <br />/var/named/chroot_sdb/dev/urandom character device system_u:object_r:urandom_device_t:s0 <br />/var/named/chroot_sdb/dev/zero character device system_u:object_r:zero_device_t:s0 <br />/var/named/data(/.*)? all files system_u:object_r:named_cache_t:s0 <br />/var/named/dynamic(/.*)? all files system_u:object_r:named_cache_t:s0 <br />/var/named/named\.ca regular file system_u:object_r:named_conf_t:s0 <br />/var/named/slaves(/.*)? all files system_u:object_r:named_cache_t:s0 <br />/var/run/bind(/.*)? all files system_u:object_r:named_var_run_t:s0 <br />/var/run/ecblp0 named pipe system_u:object_r:cupsd_var_run_t:s0 <br />/var/run/initctl named pipe system_u:object_r:initctl_t:s0 <br />/var/run/named(/.*)? all files system_u:object_r:named_var_run_t:s0 <br />/var/run/ndc socket system_u:object_r:named_var_run_t:s0 <br />/var/run/systemd/initctl/fifo named pipe system_u:object_r:initctl_t:s0 <br />/var/run/unbound(/.*)? all files system_u:object_r:named_var_run_t:s0 <br />/var/named/chroot/usr/lib64 = /usr/lib<br />/var/named/chroot/lib64 = /usr/lib<br />/var/named/chroot/var = /var<br />[root@ns01 ~]#</p>
<p><span style="font-weight: 400;">Изменим тип контекста безопасности для каталога /etc/named: </span><em><span style="font-weight: 400;">sudo chcon -R -t named_zone_t /etc/named</span></em></p>
<p>[root@ns01 ~]# sudo chcon -R -t named_zone_t /etc/named<br />[root@ns01 ~]# ls -laZ /etc/named<br />total 28<br />drw-rwx---. 3 root named system_u:object_r:named_zone_t:s0 121 Sep 6 13:09 .<br />drwxr-xr-x. 87 root root system_u:object_r:etc_t:s0 8192 Sep 6 13:09 ..<br />drw-rwx---. 2 root named unconfined_u:object_r:named_zone_t:s0 56 Sep 6 13:09 dynamic<br />-rw-rw----. 1 root named system_u:object_r:named_zone_t:s0 784 Sep 6 13:09 named.50.168.192.rev<br />-rw-rw----. 1 root named system_u:object_r:named_zone_t:s0 610 Sep 6 13:09 named.dns.lab<br />-rw-rw----. 1 root named system_u:object_r:named_zone_t:s0 609 Sep 6 13:09 named.dns.lab.view1<br />-rw-rw----. 1 root named system_u:object_r:named_zone_t:s0 657 Sep 6 13:09 named.newdns.lab<br />[root@ns01 ~]#</p>
<p><span style="font-weight: 400;">Попробуем снова внести изменения с клиента:</span></p>
<p>[vagrant@client ~]$ nsupdate -k /etc/named.zonetransfer.key<br />&gt; server 192.168.50.10<br />&gt; zone ddns.lab<br />&gt; update add www.ddns.lab. 60 A 192.168.50.15<br />&gt; send<br />&gt; quit<br />[vagrant@client ~]$</p>
<p><span style="font-weight: 400;">Отработало без ошибок.</span></p>
<img width="730" height="518" alt="image" src="https://github.com/user-attachments/assets/44c97fe3-b7b6-4395-90c3-2ab3037b6c49" />
<p>&nbsp;</p>
<p><span style="font-weight: 400;">Видим, что изменения применились. Попробуем перезагрузить хосты и ещё раз сделать запрос с помощью dig: </span></p>
<p>[vagrant@client ~]$ dig @192.168.50.10 www.ddns.lab</p>
<p>; &lt;&lt;&gt;&gt; DiG 9.16.23-RH &lt;&lt;&gt;&gt; @192.168.50.10 www.ddns.lab<br />; (1 server found)<br />;; global options: +cmd<br />;; Got answer:<br />;; -&gt;&gt;HEADER&lt;&lt;- opcode: QUERY, status: NOERROR, id: 3194<br />;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1</p>
<p>;; OPT PSEUDOSECTION:<br />; EDNS: version: 0, flags:; udp: 1232<br />; COOKIE: 18a1a49e02748e7f0100000068bc441ff8248138b7ddfbb1 (good)<br />;; QUESTION SECTION:<br />;www.ddns.lab. IN A</p>
<p>;; ANSWER SECTION:<br />www.ddns.lab. 60 IN A 192.168.50.15</p>
<p>;; Query time: 4 msec<br />;; SERVER: 192.168.50.10#53(192.168.50.10)<br />;; WHEN: Sat Sep 06 14:24:31 UTC 2025<br />;; MSG SIZE rcvd: 85</p>
<p>[vagrant@client ~]$</p>
<p><span style="font-weight: 400;">После перезагрузки настройки сохранились.&nbsp;</span></p>
<p><span style="font-weight: 400;">Важно, что мы не добавили новые правила в политику для назначения этого контекста в каталоге. Значит, что при перемаркировке файлов контекст вернётся на тот, который прописан в файле политики.</span></p>
<p><span style="font-weight: 400;">Для того, чтобы вернуть правила обратно, можно ввести команду: </span><em><span style="font-weight: 400;">restorecon -v -R /etc/named</span></em></p>
<p><span style="font-weight: 400;">Задание завершено.</span></p>
