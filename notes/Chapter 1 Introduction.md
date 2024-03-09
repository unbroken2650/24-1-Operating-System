---


---

<h1 id="w1-1">W1-1</h1>
<p><strong>Objectives</strong></p>
<ul>
<li>
<p>General organization of a computer system &amp; the role of interrupts</p>
</li>
<li>
<p>Components in a modern multiprocessor computer system</p>
</li>
<li>
<p>The transition form user mode to kernel mode</p>
</li>
<li>
<p>How OSs are used in various computing environments</p>
</li>
<li>
<p>Exs of free and open-source OSs</p>
</li>
</ul>
<h2 id="what-operating-systems-do">What Operating Systems Do</h2>
<p>Computer System = Hardware + OS + Application programs + User</p>
<h3 id="operating-systemos">Operating System(OS)</h3>
<p>컴퓨터의 하드웨어를 제어하고 다양한 사용자와 다양한 App에 맞추어 편성하는 소프트웨어</p>
<h3 id="user-view">User View</h3>
<p><img src="https://i.imgur.com/ZDZJMiR.png" alt="Untitled"></p>
<p>User’s View는 컴퓨터가 사용되는 인터페이스에 따라 달라진다. 최근 모바일 기기의 사용이 늘면서 터치 스크린이나 음성 인식이 도입됨</p>
<p>임베디드 컴퓨터의 경우 User View가 없음</p>
<h3 id="system-view">System View</h3>
<p>OS는 <strong>자원 할당자</strong> + 에러를 막고 컴퓨터의 부적절한 사용을 막기 위해 프로그램의 실행을 관리하는 <strong>제어 프로그램</strong></p>
<h3 id="definition">Definition</h3>
<ul>
<li>
<p>컴퓨팅 시스템을 원활히 사용하기 위한 방법</p>
</li>
<li>
<p>컴퓨터에서 항상 돌아가는 <strong>Kernel</strong> &amp; <strong>System Programs</strong></p>
</li>
<li>
<p>최근 모바일 장치들은 App 개발자들에게 부가적인 기능을 제공하는 <strong>Middleware</strong>도 포함하고 있다</p>
</li>
</ul>
<h1 id="computer-system-organization">Computer-System Organization</h1>
<p><img src="https://i.imgur.com/jLREcqE.png" alt="Untitled"></p>
<p>현대 컴퓨터 시스템 = 하나 이상의 CPU와 수많은 Device Controller들이 각 요소와 메모리를 공유할 수 있는 <strong>Bus</strong>로 연결되어 있음<br>
각 Device Controller들은 특정한 장치를 담당함<br>
&amp; OS에는 Device Controller마다 <strong>Device Driver</strong>가 있음<br>
Device Controller를 이해하고 OS가 동일한 인터페이스로 장치를 이용하도록 만듬</p>
<h2 id="interrupts">Interrupts</h2>
<p><img src="https://i.imgur.com/0qz5qK6.png" alt="enter image description here"><br>
하드웨어는 CPU에게 신호를 보내기 위해 언제나 Interrupt를 발생 (주로 시스템 버스를 사용)<br>
ex) Device Controller가 동작을 마쳤을 때 Device Driver에게 알려주는 동작<br>
CPU가 Interrupt를 받으면 하던 동작을 멈추고 interrupt의 서비스 루틴을 실행한다.</p>

