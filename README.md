import streamlit as st
import google.generativeai as genai

st.set_page_config(page_title="CAPACITI AI Assistant", layout="wide")
st.title("🤖 AI-Powered Workplace Productivity Assistant")
st.caption("For Admin Professionals | Email + Meeting + Task Planner")

with st.sidebar:
    api_key = st.text_input("Enter Gemini API Key (free at aistudio.google.com)", type="password")
    st.markdown("**Responsible AI:** ✅ No data stored | ✅ Verify before sending")

if not api_key:
    st.warning("Add API key in sidebar to start")
    st.stop()

genai.configure(api_key=api_key)
model = genai.GenerativeModel('gemini-1.5-flash')
def gen(p): return model.generate_content(p).text

tab1, tab2, tab3 = st.tabs(["📧 Email Generator", "📝 Meeting Summarizer", "📅 Task Planner"])

with tab1:
    tone = st.selectbox("Tone", ["Formal", "Informal", "Persuasive", "Friendly"])
    audience = st.selectbox("Audience", ["Client", "Manager", "Team"])
    topic = st.text_input("Email about?")
    context = st.text_area("Context details")
    if st.button("Generate Email", type="primary"):
        prompt = f"Role: SA business communication expert. Audience: {audience}, Tone: {tone}. Topic: {topic}, Context: {context}. Task: Write 3 subject options + body <150 words + CTA. Professional."
        st.success(gen(prompt))
        st.warning("⚠️ AI-generated - review before sending.")

with tab2:
    notes = st.text_area("Paste meeting notes", height=250)
    if st.button("Summarize", type="primary"):
        prompt = f"Role: Business analyst. Task: From notes, extract: 1) 3-bullet summary 2) Key Decisions 3) Action Items table | Owner | Task | Deadline | 4) Next Steps. Notes: {notes}"
        st.success(gen(prompt))

with tab3:
    tasks = st.text_area("List your tasks today")
    hours = st.slider("Available hours", 4, 12, 8)
    if st.button("Create Plan", type="primary"):
        prompt = f"Role: Productivity coach using Eisenhower Matrix. Tasks: {tasks}, Hours: {hours}. Create time-blocked schedule 9-5, Top 3 MITs, 2 optimization tips."
        st.success(gen(prompt))
        streamlit
google-generativea
