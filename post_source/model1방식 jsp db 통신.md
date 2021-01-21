~~~

<%@page language="java contentType="text/json; charset=utf-8%>
<%@page import="java.util.ArrayList%>
<%@page import="javax.naming.InitialContext"%>
<%@page import="java.sql.*" %>

<%
ArrayList<String> colArr = new ArrayList<String>();

Connection conn = null;
PreparedStatement pstmt = null;
ResultSet rs = null;
try{
	InitialContext ctxt = new InitialContext();
	String driver = "org.postgresql.Driver"; // 접속해야하는 DB의 Driver 명칭을 넣으면된다
	String url = "jdbc:postgresql://<url>:5432/<dbname>;
	Class.forName(driver).newInstance();
	conn = DriverManager.getConnection(url, <DB_USERNAME>, <DB_USERPASSWORD>);

	StringBuffer demQuery = new StringBuffer();
	demQuery.append("select <column> from <table>");
	pstmt = conn.prepareStatement(demQuery.toString());
	rs = pstmt.executeQuery();
	while(rs.next()){
		colArr.add(rs.getString(<column>));
	}
	int loopCnt = colArr.size();
%>
[
<%
	for(int i=0; i<loopCnt; i++){
		if(i<loopCnt-1) {
			%>
			{"<column>":"<%=colArr.get(i) %>"},
			<%
			continue;
		}
		%>
		{"<column>":"<%=colArr.get(i) %>"}
		<%
	}
%>
]
<%
	} catch(Exception e){
		e.printStackTrace();
	} finally{
		rs = null;
		pstmt = null;
		conn.close();
		conn = null;
	}
%>
~~~