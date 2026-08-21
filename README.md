# AI-Workplace-Productivity-Assistant
AI-powered workplace productivity assistant for email generation, meeting summarization, task planning, research assistance, and workplace automation.
/prompts.md
/docs.md
/screenshots
import streamlit as st
import os

st.set_page_config(page_title="AI Workplace Assistant", layout="wide")
st.title("AI-Powered Workplace Productivity Assistant")
st.caption("Built for CAPACITI | Automates emails, meetings & planning")

tab1, tab2, tab3 = st.tabs(["1. Smart Email Generator", "2. Meeting Summarizer", "3. Task Planner"])

with tab1:
    st.subheader("Smart Email Generator")
    tone = st.selectbox("Tone", ["Formal", "Informal", "Persuasive"])
    audience = st.selectbox("Audience", ["Client", "Manager", "Team"])
    goal = st.text_area("What is the email about?")
    if st.button("Generate Email"):
        prompt = f"Role: Expert business writer. Audience: {audience}, Tone: {tone}. Goal: {goal}. Task: Write subject + email body <150 words with clear CTA. SA professional context."
        st.info(f"**Prompt Used:** {prompt}")
        st.success("**[AI Output - Connect your Gemini/OpenAI API here]** \n\n Subject: ... \n Body: ...")
        st.warning("⚠️ Disclaimer: AI-generated. Please review before sending.")

with tab2:
    st.subheader("Meeting Notes Summarizer")
    notes = st.text_area("Paste lengthy meeting notes", height=200)
    if st.button("Summarize"):
        prompt = f"Role: Meeting analyst. Extract: 1) 3-sentence summary 2) Key Decisions 3) Action Items (Owner-Task-Deadline) 4) Deadlines. Notes: {notes}"
        st.info(f"**Prompt Used:** {prompt}")
        st.json({"Summary": "...", "Decisions": "...", "Action Items": "..."})

with tab3:
    st.subheader("AI Task Planner")
    tasks = st.text_area("List your tasks for today")
    if st.button("Create Plan"):
        prompt = f"Role: Productivity coach using Eisenhower Matrix. Input: {tasks}. Task: Prioritize by Urgent/Important, create time-blocked schedule, suggest 2 optimization tips."
        st.info(f"**Prompt Used:** {prompt}")
        st.success("Your structured daily plan will appear here...")
        import streamlit as st
import google.generativeai as genai
import os

# --- CONFIG ---
st.set_page_config(page_title="CAPACITI AI Assistant", layout="wide", page_icon="🤖")
st.title("🤖 AI-Powered Workplace Productivity Assistant")
st.caption("CAPACITI Project | Automates Emails, Meetings & Task Planning")

# Get API Key from Streamlit Secrets or sidebar
with st.sidebar:
    st.header("Setup")
    api_key = st.text_input("Enter Gemini API Key", type="password", help="Get free key from aistudio.google.com")
    st.info("Get key: aistudio.google.com > Get API Key")
    st.markdown("---")
    st.subheader("Responsible AI")
    st.write("✅ No data stored\n✅ Human validation required\n✅ Disclaimer shown on outputs")

if not api_key:
    st.warning("Please enter your Gemini API Key in the sidebar to start.")
    st.stop()

genai.configure(api_key=api_key)
model = genai.GenerativeModel('gemini-1.5-flash')

def generate(prompt):
    try:
        response = model.generate_content(prompt)
        return response.text
    except Exception as e:
        return f"Error: {e}"

# --- TABS ---
tab1, tab2, tab3 = st.tabs(["📧 Email Generator", "📝 Meeting Summarizer", "📅 Task Planner"])

with tab1:
    st.subheader("1. Smart Email Generator")
    col1, col2 = st.columns(2)
    with col1:
        tone = st.selectbox("Tone", ["Formal", "Informal", "Persuasive"], key="tone")
        audience = st.selectbox("Audience", ["Client", "Manager", "Team"], key="aud")
    with col2:
        goal = st.text_input("Email Purpose", placeholder="e.g., Follow up on project proposal")
        context = st.text_area("Context details", placeholder="e.g., Meeting was yesterday, client name is Mr. Dlamini")

    if st.button("Generate Email", type="primary"):
        full_prompt = f"""
        Role: You are an expert South African business communication specialist.
        Context: Audience is {audience}, Tone must be {tone}.
        Task: Write a professional email.
        Purpose: {goal}
        Details: {context}
        Constraints: Under 150 words, include clear subject line, greeting, CTA, and professional closing. No jargon.
        Format: Subject: [subject] \n\n Body: [body]
        """
        with st.spinner("Generating..."):
            result = generate(full_prompt)
        st.success(result)
        st.warning("⚠️ AI Disclaimer: This is AI-generated. Please review for accuracy before sending.")
        st.caption(f"Prompt used: {full_prompt}")

with tab2:
    st.subheader("2. Meeting Notes Summarizer")
    notes = st.text_area("Paste lengthy meeting notes here", height=250, placeholder="Paste your meeting transcript/notes...")
    if st.button("Summarize Notes", type="primary"):
        if not notes:
            st.error("Paste notes first")
        else:
            full_prompt = f"""
            Role: You are a senior business analyst.
            Task: Analyze these meeting notes and extract structured information.
            Requirements:
            1. Concise Summary (3 sentences)
            2. Key Decisions Made
            3. Action Items in format: Owner - Task - Deadline
            4. Dead
