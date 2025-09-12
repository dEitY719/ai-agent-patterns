# 총 상금 5000만원! 🚀 AI 에이전트, '슬슬' 시작해볼까요?

안녕하세요, 슬시인 여러분! 👋  
혹시 요즘 이런 생각 안 해보셨나요?  
"챗봇은 이제 너무 흔한데... 내 업무를 진짜 '대신' 해주는 AI 비서 없을까?"  
"단순 반복 업무, 이제 AI한테 맡기고 나는 더 창의적인 일에 집중하고 싶다!"

바로 그 생각, `슬슬 AIdea 2025`에서 현실로 만들 기회가 찾아왔어요!

## 💰 상금만 무려 5,000만원! 근데... AI 에이전트, 그거 어떻게 만드는 건데요?

'슬슬 AIdea 2025'는 여러분의 반짝이는 AI 아이디어로 업무 혁신을 이끌어내는 **AI 에이전트 개발 경진대회**예요.

- 🏆 **총 상금**: 5,000만원 (1등 무려 2,500만원!)
- 🗓️ **개발 기간**: 6주
- 🎯 **평가 기준**: 아이디어 혁신성, 기술 완성도, 비즈니스 임팩트 등

솔직히 상금만 봐도 가슴이 뛰는데, 한편으로는 걱정도 되시죠?
"AI 에이전트는 처음인데...", "새로운 거 배우기 너무 빡센데..."

괜찮아요! 그 마음 다 알아요. 하지만 **AI 에이전트는 더 이상 선택이 아닌 필수**가 되어가고 있어요.

혹시 **코닥 모멘트**라고 들어보셨나요? 세계 최초로 디지털카메라를 개발하고도 필름 시장에 안주하다가 역사 속으로 사라진 코닥의 이야기죠. 스마트폰 혁신에 적응하지 못했던 노키아도 마찬가지고요.

지금 AI가 만들어내는 변화의 물결은 그때보다 훨씬 거대해요. 이 흐름에 올라타지 않으면, 우리도 모르는 사이에 '코닥'이나 '노키아'가 될 수 있어요. 😱 **지금이 바로 AI 에이전트라는 새로운 파도에 올라탈 절호의 기회예요!**

## 🤖 "나도 AI 에이전트 만들 수 있다!" - 걱정 마세요, 예제 코드가 있잖아요!

"그래, 중요성은 알겠어. 근데 막막하다고!" 하시는 분들을 위해, 저희가 아주 특별한 **Google Colab 스타일의 예제 코드**를 준비했어요.

이 예제는 그냥 'Hello World' 수준이 아니에요. **스스로 생각하고, 검색하고, 판단해서 똑똑한 답변을 찾아내는 'Corrective RAG'라는 AI 에이전트**를 직접 만들어보는 거예요.

### 🤔 잠깐, 이 AI 에이전트는 뭘 하는 친구인가요?

👩‍🦱: "내가 가진 회사 규정이나 논문 같은 전문 문서를 학습시켜서 답변하는 챗봇을 만들고 싶어!"  
👨‍🦲: "만약 내 문서에 답이 없으면? 스스로 구글 검색까지 해서 알려주면 대박이겠다!"  
🧔🏾‍♀️: "심지어 검색한 정보가 질문과 관련 없는 것 같으면, '이건 좀 아닌데?' 하고 질문을 바꿔서 다시 검색하면 완전 똑똑한 비서 아니야?"

네, 맞아요! 이 모든 과정을 자동화한 게 바로 예제 코드의 정체예요. 복잡해 보이지만, 사실은 **검색 → 평가 → 답변 → 재검색** 이라는 작은 단계를 연결해 에이전트처럼 행동하게 만든 거랍니다.

"별거 없네? 나도 할 수 있겠는데?" 라는 생각이 들게 하는 것이 예제 코드의 목표예요! 😉

## 💻 코드로 배우는 AI 에이전트의 생각 프로세스

자, 이제 코드를 살짝 엿보면서 AI 에이전트가 어떻게 생각하고 행동하는지 따라가 봐요. 전체 코드는 GitHub에서 클론 받아 직접 실행해볼 수 있어요!

```bash
git clone https://github.com/dEitY719/ai-agent-patterns.git
```

### 1단계: 정보 수집가, `retrieve_node` 🕵️‍♀️

AI 에이전트의 첫 번째 행동은 **정보를 찾아오는 것**이에요. 마치 탐험가가 자료를 수집하는 것과 같죠.

- **코드 역할**: "이 정보, 우리 내부 도서관(DB)에서 찾을까, 아니면 인터넷(Web)에서 찾을까?"를 결정하고 자료를 가져와요.
    
- **비유**:
    - `source == "db"`: 탐험가가 **도서관**에 가서 책을 꺼내 와요.
    - `source == "web"`: 탐험가가 **인터넷 카페**에 가서 최신 소문을 모아 와요.

```python
def retrieve_node(state: CorrectiveAgentState, config: RunnableConfig) -> Dict[str, Any]:
    """문서 검색 (DB 또는 웹)"""
    source = state.get("search_source", "db")
    print(f"{source.UPPER()} 검색")

    if source == "db":
        retriever = config["configurable"]["retriever"]
        search_results = retriever.get_relevant_documents(state["current_query"])
    else:  # web
        search_tool = TavilySearch(k=5)
        web_results = search_tool.invoke(state["current_query"])
        # ... 웹 결과를 Document 형태로 변환 ...
    return {"search_results": search_results}
```

### 2단계: 심사위원, `grade_node` 🧐

정보를 가져왔다고 끝이 아니에요. "이 자료, 진짜 쓸모 있는 거 맞아?" 라고 평가해야죠.

- **코드 역할**: 가져온 문서들이 사용자의 질문과 얼마나 관련 있는지 **코사인 유사도**로 점수를 매겨요.
    
- **비유**: **채용 면접관**이 지원자의 이력서(`search_results`)가 우리 회사 인재상(`user_question`)과 맞는지 심사하는 것과 같아요. 기준 점수(0.5)를 넘어야 합격!

```Python
def grade_node(state: CorrectiveAgentState, config: RunnableConfig) -> Dict[str, Any]:
    """문서 관련성 평가 (DB/웹 공통)"""
    # ... 질문과 문서 내용을 숫자 벡터(embedding)로 변환 ...
    # ... 코사인 유사도로 평균 점수 계산 ...
    avg_similarity = np.mean(similarities)
    docs_are_relevant = avg_similarity >= 0.5
    
    print(f"  평균 유사도: {avg_similarity:.3f}")
    print(f"  관련성 판정: {'관련' if docs_are_relevant else '비관련'}")
    
    return {"relevance_score": avg_similarity, "docs_are_relevant": docs_are_relevant}
```

### 3단계: 보고서 작성 전문가, `generate_answer_node` ✍️

이제 합격한 자료들을 바탕으로 **실제 답변을 만들어낼 시간**이에요.

- **코드 역할**: 심사를 통과한 문서들을 참고 자료(context)로 삼아 LLM(e.g., GPT-4o-mini)이 최종 답변을 생성해요.
    
- **비유**: 똑똑한 `연구원(LLM)`이 조사원들이 모아온 `참고 자료(context)`를 바탕으로 질문에 대한 보고서를 작성하는 과정이에요.

```Python
def generate_answer_node(state: CorrectiveAgentState, config: RunnableConfig) -> Dict[str, Any]:
    """답변 생성"""
    llm = ChatOpenAI(model=config["configurable"]["model_name"], temperature=0)
    context = "\n\n".join([doc.page_content for doc in state["search_results"]])
    
    # ... 프롬프트 템플릿 설정 ...
    response = (prompt | llm).invoke({"context": context, "question": state["user_question"]})
    return {"final_answer": response.content}
```

### 4단계: 전략가, `rewrite_query_node` 🗺️

만약 심사 단계에서 "자료가 별로 관련 없는데?"라는 결과가 나오면 어떡할까요? 포기할 순 없죠!

- **코드 역할**: 원래 질문을 더 좋은 검색 결과가 나올 만한 **새로운 검색어로 다시 작성**해요.
    
- **비유**: 탐험가가 길을 잃었을 때, **전문가(LLM)가 더 좋은 길을 알려주는 새로운 지도를 그려주는 것**과 같아요. "이번엔 도서관 말고 인터넷에서 찾아봐!"라며 검색 장소도 바꿔주죠.

```Python
def rewrite_query_node(state: CorrectiveAgentState, config: RunnableConfig) -> Dict[str, Any]:
    """쿼리 재작성"""
    llm = ChatOpenAI(model=config["configurable"]["model_name"], temperature=0)
    # ... 프롬프트로 더 나은 검색어 생성 요청 ...
    response = (prompt | llm).invoke({"question": state["user_question"]})
    
    return {
        "current_query": response.content,
        "retry_count": state["retry_count"] + 1,
        "search_source": "web",  # 재작성 후에는 웹 검색
    }
```

### 최종 단계: 모든 것을 관장하는 지휘 본부, `CorrectiveRAG` 클래스 🏢

위에서 살펴본 모든 행동(노드)들을 한데 모아 **하나의 워크플로우로 묶어주는 역할**을 해요.

- **`_build_workflow`**: "자료를 찾고 → 평가해서 → 관련 있으면 답변 생성, 없으면 질문 재작성 후 다시 검색" 같은 **행동 순서(지도)**를 그려요.
    
- **`setup_documents`**: 우리가 가진 PDF, 텍스트 파일 등으로 **내부 도서관(DB)을 만들어요.**
    
- **`ask`**: 사용자가 질문을 던지면, 이 모든 **워크플로우를 실행시켜 최종 답변을 찾아와요.**
    

```Python
class CorrectiveRAG:
    def __init__(self, model_name="gpt-4o-mini"):
        # ... 모델, 워크플로우 초기화 ...

    def _build_workflow(self):
        # ... StateGraph로 노드와 조건부 엣지를 연결해 워크플로우 구성 ...

    def setup_documents(self, input_files: List[str]):
        # ... 파일 로드, 텍스트 분할, 벡터DB 생성 ...

    def ask(self, question: str) -> str:
        # ... 워크플로우 실행 및 최종 답변 반환 ...

# 이렇게 간단하게 쓸 수 있어요!
agent = CorrectiveRAG()
agent.setup_documents(["내_전문_문서.pdf"])
answer = agent.ask("피싱 메일 대응 방안 알려줘.")
print(answer)
```

어때요? 생각보다 할 만하지 않나요? 이 예제 코드만 잘 이해해도, 여러분만의 멋진 AI 에이전트를 만들 수 있는 기본기는 갖춘 셈이에요!

## 🎉 이제 당신의 아이디어를 펼칠 시간입니다!

`슬슬 AIdea 2025`는 단순히 코딩 실력을 겨루는 대회가 아니에요. 여러분의 아이디어가 어떻게 AI 기술과 만나 우리의 업무를 더 스마트하게 만들 수 있는지 보여주는 축제의 장입니다.

- **"AI, 잘 모르는데..."** 👉 괜찮아요! 이 예제와 함께라면 누구나 시작할 수 있어요.
- **"아이디어만 있는데..."** 👉 최고의 파트너를 만나 현실로 만들어보세요! (Best Partner상도 있어요!)

6주라는 시간 동안 여러분의 잠재력을 폭발시켜 보세요. 지금 시작하지 않으면, 변화의 흐름에 뒤처질지도 몰라요. 코닥과 노키아의 전철을 밟지 말고, AI 에이전트 시대의 주인공이 되어보는 건 어떨까요?

**당신의 AIdea가 세상을 바꿀 수 있습니다. 지금 바로 도전하세요!**

➡️ **[슬슬 AIdea 2025 참가 신청 바로가기]**

## 붙이지 못한 편지
여기까지 읽었다면 관심은 있는 개발자라고 생각해.
`(동기 부여되는)` 진짜 마음의 소리를 한마디 할게.

회사 목숨걸고 일해서 상위고과 받아야 `가` 천, `나` 오백이야. (받아봐서 다들 알잖아. 물론 `핵인` 열외)

AI 공부해. 공부해서 더 좋은 곳 찾아가.
AI로 집에서 여러가지 만들 수 있어.
`ㄱㄴ` 노른자 먹고 싶지? 애들 `SNU` 보내고 싶지?

AI 활용하면 다 이룰 수 있어. 내가 산 증인이야. 20000! ㅋ
