# candidate
지역구 후보
# 후보 조회
```db
<%

	StringBuffer sb = new StringBuffer();
	sb.append("select mem.*, pt.*, substr(mem.M_JUMIN,1,6) as f_jumin, substr(mem.M_JUMIN,7,13) as b_jumin, ")
		.append(" case mem.P_SCHOOL")
		.append("    when '1' then '고졸' ")
		.append("    when '2' then '학사' ")
		.append("    when '3' then '석사' ")
		.append("    when '4' then '박사' ")
		.append("    end p_schooln        ")
		.append("from TBL_MEMBER_202005 mem, TBL_PARTY_202005 pt ")
		.append("where mem.P_CODE = pt.p_code                    ")
		.append("order by mem.p_code                             ");
	
	String sql =sb.toString();

	Connection conn = DBConnect.getConnection();
   	PreparedStatement ps = conn.prepareStatement(sql);
   	ResultSet rs = ps.executeQuery();
   	
%>
```
후보번호,성명,소속정당,지역구,대표전화을 조회하고 학력은 1일 경우 고졸, 2일 경우 학사,3일 경우 석사, 4일 경우 박사로 표시한다<br>
주민번호는 000000-0000000으로 표시하여 출력한다<br>
![image](https://user-images.githubusercontent.com/102035198/210191869-c4f970fd-c884-4bc3-a3f7-30e9f42daa78.png)<br>
