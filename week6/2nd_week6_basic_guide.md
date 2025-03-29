1. 1. ## 준비

      ------

      다음 실습 코드 위에서 진행하시면 됩니다:

      - 전체 코드

        ```python
        import base64
        import streamlit as st
        
        from langchain_openai import ChatOpenAI
        from langchain_core.messages import HumanMessage
        
        st.title("Fashion Recommendation Bot")
        model = ChatOpenAI(model="gpt-4o-mini")
        if image := st.file_uploader("본인의 전신이 보이는 사진을 올려주세요!", type=['png', 'jpg', 'jpeg']):
            st.image(image)
            image = base64.b64encode(image.read()).decode("utf-8")
            with st.chat_message("assistant"):
                message = HumanMessage(
                    content=[
                        {"type": "text", "text": "사람의 전신이 찍혀있는 사진이 한 장 주어집니다. 이 때, 사진 속 사람과 어울리는 옷 및 패션 스타일을 추천해주세요."},
                        {
                            "type": "image_url",
                            "image_url": {"url": f"data:image/jpeg;base64,{image}"},
                        },
                    ],
                )
                result = model.invoke([message])
                response = result.content
                st.markdown(response)
        ```

      ## 목표

      ------

      이전 이미지 관련 실습에서는 사용자가 이미지를 업로드하면 하드 코딩 된 prompt와 함께 GPT로 보내 답변을 받는 챗봇을 구현했습니다. 이번에는 다음과 같이 기능을 확장하는 것이 과제의 목표입니다:

      - [ ]  여러 이미지를 입력으로 받기
        - 기존에는 이미지 한 장만을 입력으로 받았다면 이번에는 다수의 사진을 입력 받을 수 있도록 만들어야 합니다.
        - Streamlit의 `st.file_uploader` [문서](https://docs.streamlit.io/develop/api-reference/widgets/st.file_uploader)를 확인하여 기존 코드에서 여러 장의 사진을 받을 수 있도록 구현해봅시다.
      - [ ]  업로드 된 이미지들을 가지고 자유롭게 질의응답 할 수 있는 챗봇 구현
        - 기존과 다르게 하드코딩 된 prompt가 아닌, 사용자로부터 질문을 입력받아 GPT에게 넘겨주는 챗봇을 구현하셔야 합니다.
        - 그리고 사용자가 여러 번 질문을 입력해도 처음 주어진 이미지들로 답변할 수 있도록 구현하셔야 합니다(이 부분은 RAG 실습 코드를 참조).
        - GPT에게 여러 개의 사진을 보내주는 부분은 [API 문서](https://platform.openai.com/docs/guides/vision)를 확인하여 구현하시면 됩니다.
      - [ ]  다음 이미지들과 질문에 대한 챗봇의 답변 생성
        - 다음 주어진 이미지들과 질문을 실제로 구현한 챗봇에게 주어졌을 때 어떤 답변이 생성되는지 영상을 녹화하셔야 합니다:
          - 이미지: 인터넷에서 강아지 사진과 고양이 사진 각각 1장씩 찾아 입력으로 쓰시면 됩니다.
          - 질문 1: 주어진 두 사진의 공통점이 뭐야?
          - 질문 2: 주어진 두 사진의 차이점이 뭐야?
        - 질문 1, 2를 차례로 입력하시면 됩니다. 즉, 챗봇과 질의응답이 두 차례 이루어져야 합니다.

      ## 제출자료

      ------

      서비스를 구현한 코드와 구동 영상을 github에 함께 올리주시면 됩니다.