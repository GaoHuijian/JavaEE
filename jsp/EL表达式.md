EL���ʽ
* ${���ʽ}
* ���ʽ:��������,��ϵ����,�߼�������ʽ,��Ԫ������ʽ

�Ĵ��������Ĵ�������

EL							JSP
-----------------------------------------------------
applicationScope					application
sessionScope					session
requestScope					request
pageScope						pageContext


* ELֻ��֪ͨtomcat��ȡ�Ĵ��������е�����,�������޸��Ĵ��������е�����
* ${EL����������,�ؼ���}
  ${�ؼ���}  :tomcat 1) pageContext 2) request 3) session 4) application



EL ���ض���
pageContext  :��ȡJSP��pageContext�л�����Ϣ
param		 : ֪ͨtomcat����request.getParmeter("������Ϣ")��ȡ�������ֵ
