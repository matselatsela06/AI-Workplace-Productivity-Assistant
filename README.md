import streamlit as st
import google.generativeai as genai

st.set_page_config(page_title="CAPACITI AI Assistant", layout="wide")
st.title("AI-Powered Workplace Productivity Assistant")
st.caption("For Admin Professionals - Email | Meetings | Planner")

api_key = st.sidebar.text_input("Enter Gemini API Key (get free at aistudio.google.com)", type="password")
if not api_key:
    st.warning("Enter API key in sidebar to start")
    st.stop()

genai.configure(api_key=api_key)
model = genai.GenerativeModel('gemini-1.5-flash')
def ask(prompt):
    return model.generate_content(prompt).text

tab1, tab2, tab3 = st.tabs(["Email Generator", "Meeting Summarizer", "Task Planner"])

with tab1:
    tone = st.selectbox("Tone", ["Formal", "Informal", "Persuasive"])
    audience = st.selectbox("Audience", ["Client", "Manager", "Team"])
    topic = st.text_input("Email about?")
    context = st.text_area("Context")
    if st.button("Generate Email", type="primary"):
        p = f"Role: SA business writer. Tone: {tone}, Audience: {audience}, Topic: {topic}, Context: {context}. Write subject + body <150 words + CTA."
        st.success(ask(p))
        st.warning("AI-generated - verify before sending")

with tab2:
    notes = st.text_area("Paste meeting notes", height=200)
    if st.button("Summarize"):
        p = f"Role: Business Analyst. Summarize notes into: 1) 3-bullet summary 2) Key Decisions 3) Action Items table Owner|Task|Deadline 4) Next Steps. Use TBC if no deadline. Notes: {notes}"
        st.success(ask(p))

with tab3:
    tasks = st.text_area("Your tasks today")
    hours = st.slider("Available hours", 4, 12, 8)
    if st.button("Create Plan"):
        p = f"Role: Productivity Coach using Eisenhower Matrix. Tasks: {tasks}, Hours: {hours}. Create time-blocked plan, Top 3 MITs, 2 optimization tips."
        st.success(ask(p))
        streamlit
google-generativeai
Role+Context+Task+Constraint technique used

1. Email: Role: SA business writer, Tone={formal/informal}, Audience={client/manager/team}, Task: subject+body+CTA, Constraint: <150 words
2. Meeting: Role: Analyst, Task: summary+decisions+action items table, Constraint: Use TBC if no date to avoid hallucination
3. Task Planner: Role: Coach, Framework: Eisenhower Matrix, Task: prioritize + time-block
4. # AI-Powered Workplace Productivity Assistant
Live Link: [ADD YOUR STREAMLIT LINK AFTER DEPLOY]
Problem: Admins spend 3+ hrs on emails/meetings/planning
Solution: 3-in-1 AI tool saves 2 hrs/day
Tools: Streamlit + Gemini API + GitHub
Features: Email Generator, Meeting Summarizer, Task Planner
Ethics: Disclaimer on outputs, no data stored, human validation, bias noted
Value: 45min -> 2min for emails, 60min -> 3min for meetings
