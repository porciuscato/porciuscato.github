mariadb ��ġ �� ����

https://www.buyprotheme.com/uninstall-mysql-from-ubuntu-and-install-mariadb/


root ��й�ȣ ����

https://websiteforstudents.com/reset-mariadb-root-password-ubuntu-17-04-17-10/



```bash
sudo netstat -antp | grep mysql
```

mysql�� ���� ������ Ȯ���� �� �ִ�.
�̶� IP�� 127.0.0.1:3306���� �Ǿ������� ����ȣ��Ʈ������ ������ �����ϴ�. �ٸ� IP������ ������ �����Ϸ��� ������ ���� �����ؾ� �Ѵ�.



```bash
sudo vi /etc/mysql/my.cnf
```

bind-address �� 0.0.0.0���� �ٲٰ� mysql�� �ٽ� ����

```bash
sudo service mysql restart
```



