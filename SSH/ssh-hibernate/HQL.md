HQL

Hibernate Query Language ��һ���������Ĳ�ѯ����.Hibernate����ṩ��.
��sql���з�װ,�﷨������Sql���,����HQL��Ҫ�����SQL���ܽ������ݿ����ݲ���.

lazy	:proxy 	:֧���ӳټ���,Ĭ��ֵ
    	:no-proxy:	:�����ô���ʽʵ����ʱ����.
	:false	:�����ӳټ���

fetch:ץȡ����
	��ʾ����ʲô�����,��ѯ��������.
	select :���ö������ķ�ʽ���й������ݲ�ѯ.
	join   :��ʾ�����������ķ�ʽ���в�ѯ. Ĭ�ϲ����������Ӳ�ѯ.
 
����������Ӳ�ѯ,�ӳټ��ؾ�ʧЧ��.	
	
��֧��select * from User
from User
select U from User as U


���ݲ��� ��0��ʼ
Query query = session.createQuery("from User u where u.name like ?");

Query query = session.createQuery("delete from User u where u.id in (:ids)"); //   :ids  ��ʾ����һ����������(�������Զ����)

query.setFirstResult(startIndex);
query.setMaxResults(pagesize);

