# AI Agent Patterns

**ai-agent-patterns**는 LangChain과 LangGraph를 활용하여 다양한 AI 에이전트 설계 패턴을 탐구하는 저장소입니다. 이 프로젝트는 단순한 도구(Tool) 사용부터 시작하여, 질문을 분류하는 라우터(Router), 그리고 여러 전문 에이전트들이 협력하는 복잡한 멀티 에이전트 시스템에 이르기까지 점진적인 학습 경험을 제공합니다.

<br/>

## 프로젝트 목표

- **LangChain 및 LangGraph 학습:** Agent 및 Graph 기반 워크플로우를 구축하는 방법을 단계별로 학습합니다.
- **Agent 패턴 이해:** 질문 라우팅, 전문 에이전트 분리, 슈퍼바이저(Supervisor) 에이전트 오케스트레이션 등 핵심 패턴을 이해합니다.
- **실용적인 예제 제공:** 실제 코드를 통해 각 패턴이 어떻게 동작하는지 직관적으로 파악할 수 있도록 돕습니다.

---

## 예제 구성 (feat: Agentic RAG 시스템 예제 4종)

**Agentic RAG 예제 4종**을 한눈에 비교하여 정리합니다.
각 시스템은 동일한 기본 기능을 제공하지만, **Router 방식**과 **Multi-Agent 구조** 적용 여부에 따라 점진적으로 복잡성과 지능 수준이 달라집니다.

| 시스템 이름                 | Router 방식 | Multi-Agent 구조 | 특징 / 설명                                                       |
| ---------------------- | --------- | -------------- | ------------------------------------------------------------- |
| **SimpleAgenticRAG**   | ❌ 없음      | ❌ 단일 Agent     | 가장 단순한 구조. 단일 Agent가 여러 도구를 직접 호출. 초급 학습용.                    |
| **RuleBasedRoutedRAG** | ✅ 규칙 기반   | ❌ 단일 Agent     | 조건문 라우팅. 질문 유형별로 고정된 핸들러에 분기. 규칙 중심의 단순 Router.               |
| **LLMRoutedRAG**       | ✅ LLM 기반  | ❌ 단일 Agent     | Router LLM이 질문을 해석해 도구 선택. 더 유연하고 지능적인 분기 가능.                 |
| **MultiAgentRAG**      | ✅/선택적     | ✅ 다중 Agent     | 각 도구가 독립 Agent로 분리, Supervisor Agent가 협업을 조율. 대규모/복합 시스템에 적합. |


### 핵심 포인트

1. **SimpleAgenticRAG**: 가장 단순한 구조, Agent와 도구 연동만으로 동작
2. **RuleBasedRoutedRAG**: 조건문 기반 분기, 질문 유형별 라우팅을 단순 규칙으로 처리
3. **LLMRoutedRAG**: Router LLM을 활용해 질문을 해석하고 더 유연하게 도구 선택
4. **MultiAgentRAG**: 각 도구를 독립 Agent로 분리, Supervisor가 협업을 관리하는 확장형 구조


> 4개의 예제를 통해 **단일 Agent → 규칙 기반 라우팅 → LLM 기반 라우팅 → Multi-Agent 협업**으로 이어지는 발전 과정을 직관적으로 확인할 수 있습니다.
> 연구/실습/서비스 규모에 따라 적합한 예제를 선택해 적용할 수 있을 것입니다. 🚀

---

## 설치 및 실행

### 1. 가상 환경 설정
```bash
python -m venv .venv
source .venv/bin/activate  # macOS/Linux
# .venv\Scripts\activate  # Windows
```

### 2. 패키지 설치
```bash
pip install -r requirements.txt
```

### 3. 프로젝트 구조
```bash
tree .
.
├── README.md
├── requirements.txt
├── .env   # ✨ API KEY를 발급받고 작성
└── src
    ├── data
    │   ├── [붙임1] 2025년 서울형 R&D 지원사업 통합공고.hwp
    │   ├── 2024학년도+2학기+온라인+수업+중간+강의평가+결과.xlsx
    │   ├── 2024_주요법령해석사례_해설집.pdf
    │   ├── [이론3] 다양한 문서 처리 Tool 구현.pptx
    │   ├── Apache_2k.log
    │   ├── customers.txt
    │   ├── Delivery-Plan-Cabinet-report-South.docx
    │   └── TuringComputing.pdf
    ├── practice_Agentic_RAG.ipynb
    └── UnderstandingDeepLearning_05_29_25_C.pdf
```

### 4\. API 키 설정

프로젝트에 필요한 API 키를 설정하기 위해 `.env` 파일을 생성합니다. 이 파일은 보안을 위해 Git에 포함되지 않도록 `.gitignore`에 추가하는 것이 좋습니다.
먼저, 아래 링크에서 각 API 키를 발급받으세요:

  * **OpenAI API 키:** [https://platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys)
  * **Tavily API 키:** [https://www.tavily.com/](https://www.tavily.com/)

발급받은 키를 사용하여 프로젝트 루트 디렉터리에 `.env` 파일을 만들고 아래와 같이 내용을 작성합니다.

```ini
OPENAI_API_KEY="sk-proj-Ap********"
TAVILY_API_KEY="tvly-eMVVz********"
```

**주의:** 따옴표는 선택 사항이지만, 키 값에 공백이나 특수 문자가 포함될 경우를 대비하여 사용하는 것을 권장합니다.

### 5. 실행 방법

`practice_Agentic_RAG.ipynb` 파일은 Jupyter Notebook 형식으로 되어 있으므로, VS Code에서 확장 프로그램을 설치하여 실행할 수 있습니다.

#### 4.1. Jupyter 확장 프로그램 설치 🧩

VS Code 좌측 메뉴에서 **확장 프로그램** 아이콘을 클릭하거나 `Ctrl+Shift+X`를 눌러 확장 프로그램 마켓플레이스를 엽니다. 검색창에 **"Jupyter"**를 입력하고 Microsoft에서 제공하는 확장 프로그램을 설치합니다.


#### 4.2. 가상 환경 활성화 🚀

VS Code의 터미널을 열고(단축키 `Ctrl+'`) 이전에 설정한 가상 환경을 활성화합니다.

* **macOS/Linux:** `source venv/bin/activate`
* **Windows:** `.\venv\Scripts\activate`


#### 4.3. Notebook 실행 💻

`src/practice_Agentic_RAG.ipynb` 파일을 클릭하여 엽니다. 파일 상단에 나타나는 'Jupyter 서버 시작' 버튼을 클릭하거나, 커널을 선택하라는 메시지가 나타나면 **"Python Environments"**에서 `venv`로 설정된 가상 환경을 선택합니다.

이제 각 셀(`In [ ]:` 으로 표시된 코드 블록) 옆의 실행 버튼을 클릭하거나 `Shift+Enter`를 눌러 코드를 순차적으로 실행하며 결과를 확인할 수 있습니다.
