Servlet��������:

ServletContext		application  �����û��������
HttpSession 		session	 �����û�
HttpServletRequest  	request 	 ��������
PageContext			page		 ��ǰҳ��
Cookie			cooki		 �û������
ServletConfig				 ����Servlet
ServletResponse				 ������Ӧ

JSP���ö���:

		out			JspWriter
		response		HttpServletResponse
		config		ServletConfig
		exception		Throwable
		page			this (HttpJspBase) ������

		pageContext		PageContext
		request		HttpServletRequest
		session		HttpSession
		application		ServletContext

EL���ʽ����:


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
