# ansible-filebeat

Bu ufak Ansible reposu, **Filebeat** uygulamasının (deb ve rpm paketlerinin) dağıtımını ve belirli bir **logstash** (isteğe bağlı Kibana veya Elasticsearch'a gönderecek şekilde de değiştirilebilir) sunucusuna log yollaması için gerekli konfigürasyonun sağlanması için yazılmıştır.



### Hazırlık

- Proje bir Ansible sunucusunda çalıştırılacağından öncelikle Ansible ve gerekli diğer paketler yüklenir.

```
sudo apt install ansible git sshpass
```

- Ardından **/opt** dizinine gidilerek repo indirilir. Repoda gerekli deb ve rpm paketleri olduğundan bu işlem biraz sürebilir.

```
cd /opt
git clone https://github.com/fthrslntgy/ansible-filebeat
cd ansible-filebeat
```



### Ayarlamalar

Repo içerisindeki IP, şifre gibi değişkenler örnek olarak girildiğinden bu değerlerin özelleştirilmesi gerekir.

- Yerele indirilen repo (**/opt/ansible-filebeat/**) içerisindeki **inventory/hosts** dosyası açılarak rpm ve deb başlığı altındaki **IP'ler, ssh user ve password değerleri ve sudo_pass değeri** ayarlanmalıdır. Örnek olması açısından ikişer satır konulmuştur, ayarlamalar yapıldıktan sonra kullanılmayacak satırlar silinebilir veya daha fazla satır eklenebilir.

```
sudo nano /opt/ansible-filebeat/inventory/hosts
```

- Daha sonra ise yine repodaki **files/filebeat.yml** dosyasında **147. satırda geçen LOGSTASH_IP** değişkeni yerine geçerli IP değeri girilmelidir.
  - Genel olarak Logstash 5044 portunu kullandığından bu değer girilmiştir. Logstash tarafında farklı bir port kullanılıyorsa aynı satırdaki bu değeri de değiştiriniz.
  - **files/filebeat.yml** dosyası Ansible ile Filebeat kurulumu yapıldıktan sonra **uzak makinede ayarlanacak olan Filebeat ayarlarıdır**. Repodaki dosyada Logstash sunucusuna log gönderilmek üzere konfigürasyonlar yapılmıştır. Eğer farklı uygulamalara (Elastic, Kibana vs.) gönderim yapılmak isteniyorsa bu dosya üzerinde gerekli değişiklikler yapılmalıdır.

```
sudo nano /opt/ansible-filebeat/files/filebeat.yml
```



### Çalıştırma

- Tüm ayarlamalar yapıldıktan sonra playbook dosyaları çalıştırılabilir. 
- Debian tabanlı işletim sistemlerine (hosts dosyasında **[deb]** başlığı altında girilen makineler) dağıtım için aşağıdaki komut çalıştırılır.

```
ansible-playbook /opt/ansible-filebeat/playbooks/deb.yml
```

- RPM tabanlı işletim sistemlerine (hosts dosyasında **[rpm]** başlığı altında girilen makineler) dağıtım için aşağıdaki komut çalıştırılır.

```
ansible-playbook /opt/ansible-filebeat/playbooks/rpm.yml
```



