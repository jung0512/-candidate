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
테이블 두개를 사용하여야 하기 때문에 테이블을 조인하여 준다<br>
후보번호,성명,소속정당,지역구,대표전화을 조회하고 학력은 1일 경우 고졸, 2일 경우 학사,3일 경우 석사, 4일 경우 박사로 표시한다<br>
주민번호는 000000-0000000으로 표시하여 출력한다<br>
```
<table class="tableList">
			<tr>
				<td>후보번호</td>
				<td>성명</td>
				<td>소속정당</td>
				<td>학력</td>
				<td>주민번호</td>
				<td>지역구</td>
				<td>대표전화</td>
			</tr>
			<%
			
			while(rs.next()){
			
			%>
			<tr>
				<td><%=rs.getString("m_no")%></td>
				<td><%=rs.getString("m_name")%></td>
				<td><%=rs.getString("p_name")%></td>
				<td><%=rs.getString("p_schooln")%></td>
				<td><%=rs.getString("f_jumin")%>-<%=rs.getString("b_jumin")%></td>
				<td><%=rs.getString("m_city")%></td>
				<td><%=rs.getString("p_tel1")%>-<%=rs.getString("p_tel2") %>-<%=rs.getString("p_tel3") %></td>
			</tr>
			<%} %>
		</table>
```
sql문을 이용하여 테이블을 출력하여준다<br>
![image](https://user-images.githubusercontent.com/102035198/210191869-c4f970fd-c884-4bc3-a3f7-30e9f42daa78.png)<br>
# 투표하기 
```
<script type="text/javascript">
	function remove() {
		alert("모든 내용을 지우고 다시 입력합니다!");
		document.vData.reset();
		document.vData.jumin.focus();
	}
	
	function checkVal(){
		
		if(!document.vData.jumin.value){
			alert("주민번호가 입력되지 않았습니다!");
			document.vData.jumin.focus();
			return false;
		}else if(!document.vData.name.value){
			alert("성명이 입력되지 않았습니다!");
			document.vData.name.focus();
			return false;			
		}else if(!document.vData.vote_num.value){
			alert("후보번호가 입력되지 않았습니다!");
			document.vData.vote_num.focus();
			return false;
		}else if(!document.vData.time.value){
			alert("투표시간이 입력되지 않았습니다!");
			document.vData.time.focus();
			return false;
		}else if(!document.vData.place.value){
			alert("투표장소가 입력되지 않았습니다!");
			document.vData.place.focus();
			return false;
		}else if(!document.vData.confirm.value){
			alert("유권자확인이 선택되지 않았습니다!");
			return false;
		}
		alert("투표하기 정보가 정상적으로 등록되었습니다!");
	}
</script>
```
입력되지 않은 칸이 있을 시 경고창을 띄우고 포커스를 이동시켜주는 유효성 검사이다.<br>
```
<table class="tableList">
				<tr>
					<td>투표하기</td>
					<td class="align-left"><input type="text" size="26" name="jumin"> 예: 8912121111111</td>
				</tr>
				<tr>
					<td>성명</td>
					<td class="align-left"><input type="text" size="20"  name="name"></td>
				</tr>
				<tr>
					<td>후보번호</td>
					<td class="align-left">
						<select name="vote_num">
							<option value="">직업선택</option>
							<option value="1">[1]김후보</option>
							<option value="2">[2]이후보</option>
							<option value="3">[3]박후보</option>
							<option value="4">[4]조후보</option>
							<option value="5">[5]최후보</option>
						</select>
					</td>
				</tr>
				<tr>
					<td>투표시간</td>
					<td class="align-left"><input type="text" size="20"  name="time"></td>
				</tr>
				<tr>
					<td>투표장소</td>
					<td class="align-left"><input type="text" size="20"  name="place"></td>
				</tr>
				<tr>
					<td>유권자확인</td>
					<td class="align-left">
						<input type="radio" id="confirm_y"  name="confirm" value="Y">
						<label for="confirm_y">확인</label>
						<input type="radio" id="confirm_n"  name="confirm" value="N">
						<label for="confirm_n">미확인</label>
					</td>
				</tr>
				<tr>
					<td colspan="2">
						<input type="submit" value="투표하기">
						<input type="button" value="다시하기" onclick="remove()">
					</td>
				</tr>
			</table>
```
주민번호, 성명, 투표시간, 투표장소는 입력창으로 만들어준다<br>
후보번호와 유권자확인은 각각 셀렉트박스와 라이오버튼으로 만들어준다<br>
![image](https://user-images.githubusercontent.com/102035198/210286882-b3948e6e-660d-46a1-aacf-23c358f009fa.png)
![image](https://user-images.githubusercontent.com/102035198/210286891-d637642b-c7c2-4141-b870-f65a0109ec0d.png)
# 투표검수조회
```
<%
    String sql = "select V_NAME ,'19'||SUBSTR(V_JUMIN,1,2)||'년'||SUBSTR(V_JUMIN,3,2)||'월'||SUBSTR(V_JUMIN,5,2)||'일생' AS V_BIRTH, "
    	+ "TRUNC(MONTHS_BETWEEN(SYSDATE, TO_DATE(SUBSTR('19'||V_JUMIN,1,8), 'YYYYMMDD'))/12) AS V_AGE, "
    	+ "CASE SUBSTR(V_JUMIN,7,1) WHEN '1' THEN '남자' WHEN '2' THEN '여자' WHEN '3' THEN '남자' WHEN '4' THEN '여자' END AS V_GENDER, "
    	+ "V_NO , SUBSTR(V_TIME,1,2) || ':' || SUBSTR(V_TIME,3,2) AS V_TIME, "
    	+ "CASE V_CONFIRM WHEN 'Y' THEN '확인' WHEN 'N' THEN '미확인' END AS V_CONFIRM "
    	+ "from TBL_VOTE_202005 " 
    	+ "where V_AREA = '제1투표장'";
    
    Connection conn = DBConnect.getConnection();
    PreparedStatement pstmt = conn.prepareStatement(sql);
    ResultSet rs = pstmt.executeQuery();
%>
```
이 페이지는 제1투표장에서 투표를 한 사람만 뜨는 페이지이다<br>
만나이를 구해주고 주민번호 뒷자리 숫자를 이용하여 성별을 나누어 주었다<br>
유권자확인이 Y면 확인 N이면 미확인으로 출력하여 준다<br>
```
<table class="tableList">
			<tr>
				<td>성명</td>
				<td>생년월일</td>
				<td>나이</td>
				<td>성별</td>
				<td>후보번호</td>
				<td>투표시간</td>
				<td>유권자확인</td>
			</tr>
			<%while(rs.next()){ %>
			<tr>
				<td><%=rs.getString(1) %></td>
				<td><%=rs.getString(2) %></td>
				<td><%=rs.getString(3) %></td>
				<td><%=rs.getString(4) %></td>
				<td><%=rs.getString(5) %></td>
				<td><%=rs.getString(6) %></td>
				<td><%=rs.getString(7) %></td>
			</tr>
			<%} %>
		</table>
```
실행화면은 이렇다<br>
![image](https://user-images.githubusercontent.com/102035198/210287364-67c3aeb7-f2da-4d4e-8d81-97863f32aed9.png)<br>
# 후보자등수
```
<%
    String sql = "select  v_no, m_name, count(*) cnt "
    			+ "from TBL_VOTE_202005 vote, TBL_MEMBER_202005 mem "
    			+ "where vote.v_no=mem.m_no and vote.V_CONFIRM = 'Y' "
    			+ "group by v_no, m_name "
    			+ "order by cnt desc "; 
    
    Connection conn = DBConnect.getConnection();
    PreparedStatement ps = conn.prepareStatement(sql);
    ResultSet rs = ps.executeQuery();
    
%>
```
후보번호와 성명, 투표건수를 count함수로 구해준다<br>
```
<table class="tableList">
				<tr>
					<td>후보번호</td>
					<td>성명</td>
					<td>총투표건수</td>
				</tr>
				<%while(rs.next()){ %>
				<tr>
					<td><%=rs.getString(1) %></td>
					<td><%=rs.getString(2) %></td>
					<td><%=rs.getString(3) %></td>
				</tr>
				<%} %>
			</table>
```
![image](https://user-images.githubusercontent.com/102035198/210287451-6fb34db0-f34e-4efe-abd1-1a2123b459c9.png)<br>
