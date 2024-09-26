import streamlit as st

GRADE_MULTIPLIERS = {
    'A': 1.0,
    'B': 0.7,
    'C': 0.4
}

def year_multiplier(year):
    age = 2024 - year
    if age <= 5:
        return 1.0
    elif age <= 10:
        return 0.8
    elif age <= 15:
        return 0.6
    elif age <= 20:
        return 0.4
    else:
        return 0.2

def weight_base_price(weight):
    if weight < 1.0:
        return 100
    elif weight < 5.0:
        return 300
    else:
        return 500

def calculate_co2_reduction(weight):
    co2_reduction_per_kg = 200
    return weight * co2_reduction_per_kg

# Streamlit app
def app():
    st.title("HP Machine Valuation Tool")

    weight = st.number_input("Enter the weight of your HP machine (in kg): ", min_value=0.0, format="%.2f")
    year = st.number_input("Enter the year your machine was manufactured (e.g., 2015): ", min_value=2000, max_value=2024)
    
    if year < 2000 or year > 2024:
        st.error("Please enter a year between 2000 and 2024.")
        return

    grade = st.selectbox("Enter the grade of your machine (A, B, C): ", options=['A', 'B', 'C'])
    working_problems = st.radio("Does the machine have any working problems?", ['Yes', 'No'])
    
    problem_multiplier = 0.7 if working_problems == 'Yes' else 1.0

    # Calculate the final machine value
    base_price = weight_base_price(weight)
    final_value = (base_price * year_multiplier(year) * GRADE_MULTIPLIERS[grade] * problem_multiplier)
    
    # Calculate CO2 reduction
    co2_reduction = calculate_co2_reduction(weight)

    # Display the results
    st.write(f"Estimated value of your HP machine: ${final_value:.2f}")
    st.write(f"Estimated CO2 reduction if sold: {co2_reduction:.2f} kg")

if __name__ == "__main__":
    app()
