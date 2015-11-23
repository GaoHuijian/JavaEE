	<%--方式1    将数据直接写到jsp页面中，如果修改需要重写编译 
	<select id="pageSize" onchange="displayData(0)">
		<option value="2" selected="selected">每页2条</option>
		<option value="3">每页3条</option>
		<option value="4">每页4条</option>
		<option value="5">每页5条</option>
	</select>
	 --%>
	 <%--方式2   jsp页面中嵌套了大量的java代码，不推荐 
	 <select id="pageSize" onchange="displayData(0)">
		 <%
		 	String pageSizeString = application.getInitParameter("pageSizeString");
		 	String[] pageSizes = pageSizeString.split(",");
		 	for(String pageSize:pageSizes){
		 %>
		 	<option value="<%=pageSize %>">每页<%=pageSize %>条</option>
		 <%		
		 	}
		 %>
	 </select>
	 --%>
	 <%--方式3 c:forEach+fn 
	 <select id="pageSize" onchange="displayData(0)">
	 	<c:forEach items="${fn:split(initParam.pageSizeString,\",\") }" var="pageSize">
	 		<option value="${pageSize }">每页${pageSize }条</option>
	 	</c:forEach>																	 	
	 </select>
	 --%>
	 <%--方式4 --%>
	 <select id="pageSize" onchange="displayData(0)">
	 	<c:forTokens items="${initParam.pageSizeString}" delims="," var="pageSize">
	 		<option value="${pageSize }">每页${pageSize }条</option>
	 	</c:forTokens>
	 </select>
																	</td>															