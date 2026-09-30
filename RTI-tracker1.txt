import streamlit as st
import pandas as pd
import random
import datetime

# --- PAGE CONFIGURATION ---
st.set_page_config(
    page_title="RTI Math Strategy Tracker (1.3.D)",
    page_icon="🧮",
    layout="wide"
)

# --- INITIALIZE SESSION STATE FOR DATA TRACKING ---
if "students" not in st.session_state:
    st.session_state.students = {}

if "current_student" not in st.session_state:
    st.session_state.current_student = "Default Student"

# Pre-populate sample student if empty
if "Default Student" not in st.session_state.students:
    st.session_state.students["Default Student"] = {
        "baseline": 40.0,
        "target_date": datetime.date(2026, 11, 20),
        "logs": [
            {
                "Week": 1,
                "Date": "2026-10-05",
                "Strategy": "Make 10 (Add)",
                "Score": "4/5",
                "Accuracy": 80.0,
                "Prompt Level": "V - Verbal Strategy Cue",
                "Primary Error": "Decomposition (split 5 as 1+4)"
            }
        ]
    }

# --- NAVIGATION SIDEBAR ---
st.sidebar.title("🧮 RTI Math Manager")
st.sidebar.markdown("**Target Standard:** TEKS 1.3.D / CCSS 1.0A.C.6")

# Student Selection / Creation
student_list = list(st.session_state.students.keys())
selected_student = st.sidebar.selectbox("Select Student / Group Member:", student_list)
st.session_state.current_student = selected_student

new_student_name = st.sidebar.text_input("Add New Student Name:")
if st.sidebar.button("Add Student") and new_student_name:
    if new_student_name not in st.session_state.students:
        st.session_state.students[new_student_name] = {
            "baseline": 0.0,
            "target_date": datetime.date.today() + datetime.timedelta(weeks=8),
            "logs": []
        }
        st.rerun()

st.sidebar.markdown("---")
navigation = st.sidebar.radio(
    "Go to Module:",
    [
        "1. SMART Goal & Curriculum Guide",
        "2. Teaching & CRA Interactive Practice",
        "3. Progress Monitoring & Diagnostics"
    ]
)

# Current Student Data Shortcut
student_data = st.session_state.students[st.session_state.current_student]

# ==============================================================================
# MODULE 1: SMART GOAL & CURRICULUM GUIDE
# ==============================================================================
if navigation == "1. SMART Goal & Curriculum Guide":
    st.title("🎯 SMART Goal & Curriculum Blueprint")
    st.caption(f"Active Profile: **{st.session_state.current_student}**")

    st.subheader("1. SMART Goal Parameters")
    col1, col2, col3 = st.columns(3)
    
    with col1:
        student_data["baseline"] = st.number_input(
            "Baseline Score (%)", 
            min_value=0.0, max_value=100.0, 
            value=float(student_data["baseline"])
        )
    with col2:
        student_data["target_date"] = st.date_input(
            "Target Date", 
            value=student_data["target_date"]
        )
    with col3:
        st.info("**Target Goal:** 80% accuracy across 4 out of 5 consecutive progress monitoring trials over 6 to 9 weeks.")

    st.markdown("""
    > **SMART Goal Statement:**  
    > By **{}**, when presented with addition and subtraction expressions within 20, **{}** will apply basic fact strategies (including making 10, decomposing to 10, and doubles/near doubles) to solve accurately with **80% accuracy across 4 out of 5 consecutive progress monitoring trials**.
    """.format(student_data["target_date"].strftime("%B %d, %Y"), st.session_state.current_student))

    st.markdown("---")
    st.subheader("2. Sequential Instructional Benchmarks")
    b1, b2, b3 = st.columns(3)
    with b1:
        st.markdown("### Benchmark 1\n**Make 10 (Addition)**")
        st.write("Given addition expressions with addends 7, 8, or 9 (e.g., 8+5), decompose second addend to make 10 and solve with 80% accuracy over 3 trials.")
    with b2:
        st.markdown("### Benchmark 2\n**Decompose to 10 (Sub)**")
        st.write("Given subtraction expressions with minuends 11-19 (e.g., 14-6), decompose subtrahend to reach 10 first and solve with 80% accuracy over 3 trials.")
    with b3:
        st.markdown("### Benchmark 3\n**Flexible Fact Fluency**")
        st.write("Given mixed addition and subtraction facts within 20, independently select and apply an efficient strategy to solve with 80% accuracy over 3 trials.")

    st.markdown("---")
    st.subheader("3. CRA Academic Progression Sequence")
    
    tab1, tab2, tab3, tab4 = st.tabs([
        "Stage 1: Concrete", 
        "Stage 2: Representational", 
        "Stage 3: Abstract", 
        "Stage 4: Mastery Probes"
    ])

    with tab1:
        st.markdown("#### Physical Movement to Construct & Bridge 10")
        st.write("- **Task 1.1 (Make 10 Addition):** Fill two ten-frames with counters (e.g., 9 and 4), move counters to complete frame 1, and state sum ($10 + 3 = 13$).")
        st.write("- **Task 1.2 (Decompose Subtraction):** Build minuend on double ten-frames (e.g., 13), remove counters to clear frame 2 (3), then remove remaining from frame 1 (2).")
        st.write("- **Task 1.3 (Rote 10-Partners):** Fluently build and state pairs that equal 10 using unifix cubes or ten-frame mats (e.g., $8+2$, $7+3$, $6+4$).")

    with tab2:
        st.markdown("#### Decompose Addends/Subtrahends Using Visual Diagrams")
        st.write("- **Task 2.1 (Pictorial Make 10):** Draw number bond splits on second addend (e.g., for $8+5$, split 5 into 2 and 3; link $8+2=10$, $10+3=13$).")
        st.write("- **Task 2.2 (Pictorial Decompose Sub):** Draw number bond splits on subtrahend (e.g., for $15-7$, split 7 into 5 and 2; $15-5=10$, $10-2=8$).")
        st.write("- **Task 2.3 (Doubles + 1 Visuals):** Use dot arrays to recognize near doubles (e.g., $6+7$ as $6+6+1$).")

    with tab3:
        st.markdown("#### Apply Mental Decomposition Steps Without Visual Prompts")
        st.write("- **Task 3.1 (Mental Make 10):** Solve $9+6$ verbally/in writing by stating: *'9 needs 1 to make 10, leaving 5, so 10 + 5 = 15.'*")
        st.write("- **Task 3.2 (Mental Decompose Sub):** Solve $14 - 8$ verbally/in writing: *'14 take away 4 is 10, take away 4 more is 6.'*")
        st.write("- **Task 3.3 (Think-Addition Sub):** Solve $13-9$ by bridging addition: *'9 + 1 = 10, plus 3 more is 13, answer is 4.'*")

    with tab4:
        st.markdown("#### Select Efficient Strategy for Facts Within 20")
        st.write("- **Task 4.1 (Strategy Selection):** Given $12-5$ or $7+8$, explain why Make 10, Decompose, or Near Doubles is best.")
        st.write("- **Task 4.2 (Error Detection):** Spot error in solved work (e.g., $13-6 \\rightarrow 13-3=10 \\rightarrow 10-4=6$ corrected to $10-3=7$).")
        st.write("- **Task 4.3 (Timed Probe):** Solve 10 mixed facts within 20 in under 4 minutes with $\\ge80\\%$ accuracy using mental strategies.")


# ==============================================================================
# MODULE 2: TEACHING & CRA INTERACTIVE PRACTICE
# ==============================================================================
elif navigation == "2. Teaching & CRA Interactive Practice":
    st.title("🧩 CRA Interactive Teaching & Practice Suite")
    st.caption("Use this module during 1-on-1 or small-group instruction to teach and practice concepts visually.")

    cra_stage = st.selectbox("Select Learning Stage:", [
        "Concrete Stage (Interactive Ten-Frames)",
        "Representational Stage (Number Bond Breakdown)",
        "Abstract & Mastery Probe Generator"
    ])

    st.markdown("---")

    # --- STAGE 1: CONCRETE ---
    if "Concrete Stage" in cra_stage:
        st.subheader("Double Ten-Frame Visualizer")
        mode = st.radio("Mode:", ["Make 10 Addition", "Decompose Subtraction"], horizontal=True)

        if mode == "Make 10 Addition":
            col1, col2 = st.columns(2)
            with col1:
                add1 = st.slider("First Addend (Target 7-9)", 7, 9, 8)
            with col2:
                add2 = st.slider("Second Addend", 1, 9, 5)

            needed = 10 - add1
            remaining = add2 - needed

            st.write(f"### Problem: **{add1} + {add2} = ?**")

            # Render Visual Ten Frames
            st.markdown("#### Ten Frame 1 (First Addend):")
            frame1 = ["🔴"] * add1 + ["⚪"] * (10 - add1)
            st.markdown(" ".join(frame1[:5]) + "  |  " + " ".join(frame1[5:]))

            st.markdown("#### Ten Frame 2 (Second Addend):")
            frame2 = ["🟡"] * add2 + ["⚪"] * (10 - add2)
            st.markdown(" ".join(frame2[:5]) + "  |  " + " ".join(frame2[5:]))

            with st.expander("Show Strategy Step-by-Step", expanded=True):
                st.write(f"1. **{add1}** needs **{needed}** more counter(s) to make 10.")
                if remaining >= 0:
                    st.write(f"2. Take **{needed}** from **{add2}**, which leaves **{remaining}**.")
                    st.write(f"3. New Equation: **10 + {remaining} = {10 + remaining}**")
                else:
                    st.write(f"2. Second addend ({add2}) isn't large enough to fill Frame 1 fully.")

        else:  # Subtraction
            col1, col2 = st.columns(2)
            with col1:
                minuend = st.slider("Minuend (11 to 19)", 11, 19, 14)
            with col2:
                subtrahend = st.slider("Subtrahend", 1, 9, 6)

            st.write(f"### Problem: **{minuend} - {subtrahend} = ?**")
            
            f1_count = 10
            f2_count = minuend - 10
            
            st.markdown("#### Ten Frame 1 (10):")
            f1 = ["🔴"] * f1_count
            st.markdown(" ".join(f1[:5]) + "  |  " + " ".join(f1[5:]))

            st.markdown(f"#### Ten Frame 2 ({f2_count}):")
            f2 = ["🔴"] * f2_count + ["⚪"] * (10 - f2_count)
            st.markdown(" ".join(f2[:5]) + "  |  " + " ".join(f2[5:]))

            with st.expander("Show Subtraction Decomposition Steps", expanded=True):
                step1_take = f2_count
                step2_take = subtrahend - step1_take
                st.write(f"1. Remove **{step1_take}** from Frame 2 to reach exactly **10**.")
                if step2_take > 0:
                    st.write(f"2. You still need to subtract **{step2_take}** more from Frame 1.")
                    st.write(f"3. **10 - {step2_take} = {10 - step2_take}**")
                else:
                    st.write(f"2. You are done! Final Answer = **{minuend - subtrahend}**")

    # --- STAGE 2: REPRESENTATIONAL ---
    elif "Representational Stage" in cra_stage:
        st.subheader("Pictorial Number Bond Diagram Generator")
        
        prob_type = st.radio("Select Strategy Model:", ["Make 10 Addition", "Decompose Subtraction"], horizontal=True)

        if prob_type == "Make 10 Addition":
            st.markdown("Solve: **8 + 5**")
            st.latex(r"8 + \underbrace{5}_{2 + 3}")
            st.latex(r"(8 + 2) + 3 = 10 + 3 = 13")
            st.info("The diagram breaks 5 into **2** (to complete 8 to 10) and **3** (the remaining part).")
        else:
            st.markdown("Solve: **15 - 7**")
            st.latex(r"15 - \underbrace{7}_{5 + 2}")
            st.latex(r"(15 - 5) - 2 = 10 - 2 = 8")
            st.info("The diagram breaks 7 into **5** (to reach 10 from 15) and **2** (the remaining part to subtract).")

    # --- STAGE 3 & 4: ABSTRACT & PROBES ---
    else:
        st.subheader("Abstract Probe & Strategy Diagnostic Generator")
        
        if st.button("Generate Random 5-Question Probe"):
            st.session_state.current_probe = []
            for _ in range(5):
                is_add = random.choice([True, False])
                if is_add:
                    a = random.choice([7, 8, 9])
                    b = random.randint(4, 9)
                    st.session_state.current_probe.append({"expr": f"{a} + {b}", "ans": a + b, "type": "Make 10 (Add)"})
                else:
                    m = random.randint(11, 18)
                    s = random.randint(minuend_sub := (m - 10 + 1), 9)
                    st.session_state.current_probe.append({"expr": f"{m} - {s}", "ans": m - s, "type": "Decompose (Sub)"})

        if "current_probe" in st.session_state:
            st.write("### Quick Probe Questions:")
            for idx, q in enumerate(st.session_state.current_probe):
                c1, c2 = st.columns([2, 3])
                with c1:
                    st.markdown(f"**Q{idx+1}:** ${q['expr']}$")
                with c2:
                    st.write(f"*Strategy Target:* {q['type']} | *Answer:* **{q['ans']}**")


# ==============================================================================
# MODULE 3: PROGRESS MONITORING & DIAGNOSTICS
# ==============================================================================
elif navigation == "3. Progress Monitoring & Diagnostics":
    st.title("📈 Progress Monitoring & Diagnostic Log")
    st.caption(f"Active Profile: **{st.session_state.current_student}**")

    # --- SECTION A: LOG NEW PROGRESS TRIAL ---
    st.subheader("1. Record Weekly Probe Result")
    with st.form("log_trial_form", clear_on_submit=True):
        col1, col2, col3, col4 = st.columns(4)
        with col1:
            week_num = st.number_input("Week #", min_value=1, max_value=12, value=len(student_data["logs"]) + 1)
            trial_date = st.date_input("Trial Date", value=datetime.date.today())
        with col2:
            focus_strat = st.selectbox("Focus Strategy", ["Make 10 (Add)", "Decompose to 10 (Sub)", "Mixed Strategies"])
            score_correct = st.number_input("Correct Items (out of 5)", min_value=0, max_value=5, value=4)
        with col3:
            prompt_lvl = st.selectbox("Prompting Hierarchy Level", [
                "I - Independent",
                "V - Verbal Strategy Cue",
                "G - Gestural/Visual Diagram",
                "P - Physical Ten-Frame Model"
            ])
        with col4:
            primary_err = st.selectbox("Primary Diagnostic Error", [
                "None / Independent",
                "Combinations to 10 Error (Forgot 10-partner)",
                "Incorrect Decomposition (Split number wrong)",
                "Bridge Lost Step (Forgot remaining part)",
                "Operation Sign Error (Added instead of subtracted)",
                "Counting on 1s Fallback (Finger counting)"
            ])

        submit_log = st.form_submit_button("Submit Trial Data")

        if submit_log:
            accuracy_val = (score_correct / 5.0) * 100.0
            new_entry = {
                "Week": int(week_num),
                "Date": trial_date.strftime("%Y-%m-%d"),
                "Strategy": focus_strat,
                "Score": f"{score_correct}/5",
                "Accuracy": accuracy_val,
                "Prompt Level": prompt_lvl,
                "Primary Error": primary_err
            }
            student_data["logs"].append(new_entry)
            st.success("Trial logged successfully!")
            st.rerun()

    st.markdown("---")

    # --- SECTION B: DATA VISUALIZATION & ANALYSIS ---
    st.subheader("2. Progress Trend Visualizer & Decision Rules")

    logs = student_data["logs"]
    if logs:
        df_logs = pd.DataFrame(logs)
        
        # Display Chart
        col_chart, col_rules = st.columns([3, 2])

        with col_chart:
            # Build Trend DataFrame including baseline
            chart_data = pd.DataFrame({
                "Trial/Week": [0] + df_logs["Week"].tolist(),
                "Accuracy (%)": [student_data["baseline"]] + df_logs["Accuracy"].tolist()
            }).set_index("Trial/Week")

            st.line_chart(chart_data)
            st.dataframe(df_logs[["Week", "Date", "Strategy", "Score", "Accuracy", "Prompt Level", "Primary Error"]], use_container_width=True)

        with col_rules:
            st.markdown("#### RTI Phase Decision Rules")
            
            # Decision Analysis
            recent_acc = df_logs["Accuracy"].tail(3).tolist()
            mastery_met = len(recent_acc) >= 3 and all(acc >= 80.0 for acc in recent_acc)
            
            if mastery_met:
                st.success("🎉 **Mastery Achieved:** ≥80% accuracy over 3 consecutive weeks!\n\n**Action:** Advance to 2nd-grade fluencies (facts within 100).")
            elif len(recent_acc) >= 3 and sum(recent_acc)/len(recent_acc) < 50.0:
                st.error("⚠️ **Inadequate Progress:** Scores consistently below goal trend.\n\n**Action:** Re-anchor with double ten-frames & practice 10-partner fluency.")
            else:
                st.info("🔄 **Continue Plan:** Keep applying current intervention stage and monitoring weekly probes.")

            st.markdown("---")
            st.markdown("#### Prompting Hierarchy Reference")
            st.markdown("""
            * **I:** Independent
            * **V:** Verbal Strategy Cue
            * **G:** Gestural / Visual Diagram
            * **P:** Physical Ten-Frame Model
            """)
    else:
        st.info("No trial data recorded yet. Use the form above to log weekly scores.")
