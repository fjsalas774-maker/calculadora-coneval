import streamlit as st

st.set_page_config(page_title="Calculadora CONEVAL", page_icon="📊", layout="centered")

st.title("📊 Calculadora de Pobreza Multidimensional")
st.write("Mide tu nivel de pobreza o vulnerabilidad según la metodología oficial del CONEVAL en México.")

LPEI = 2300.0  
LPI  = 4500.0  

def evaluar_pobreza(ingreso, carencias):
    if ingreso < LPEI and carencias >= 3:
        return "Pobreza Extrema", "error"
    elif ingreso < LPI and carencias >= 1:
        return "Pobreza Moderada", "warning"
    elif ingreso >= LPI and carencias >= 1:
        return "Vulnerable por Carencias Sociales", "info"
    elif ingreso < LPI and carencias == 0:
        return "Vulnerable por Ingresos", "info"
    else:
        return "No Pobre y No Vulnerable", "success"

st.subheader("1. Ingreso Económico")
ingreso = st.number_input("Ingreso mensual per cápita del hogar ($ MXN):", min_value=0.0, value=3000.0, step=100.0)

st.subheader("2. Carencias Sociales")
st.write("Selecciona las carencias que se presentan en tu hogar:")

c1 = st.checkbox("Rezago educativo en el hogar")
c2 = st.checkbox("Falta de acceso a servicios de salud")
c3 = st.checkbox("Falta de acceso a la seguridad social")
c4 = st.checkbox("Deficiencia en calidad y espacios de la vivienda")
c5 = st.checkbox("Falta de servicios básicos en la vivienda")
c6 = st.checkbox("Falta de acceso al alimentación nutritiva y de calidad")

total_carencias = sum([c1, c2, c3, c4, c5, c6])

if st.button("Calcular mi Situación", use_container_width=True):
    resultado, estado = evaluar_pobreza(ingreso, total_carencias)
    
    st.markdown("---")
    st.subheader("Resultado de tu Evaluación:")
    st.write(f"• **Total de Carencias Sociales:** {total_carencias} de 6")
    
    if estado == "error":
        st.error(f"**Clasificación:** {resultado}")
    elif estado == "warning":
        st.warning(f"**Clasificación:** {resultado}")
    elif estado == "info":
        st.info(f"**Clasificación:** {resultado}")
    else:
        st.success(f"**Clasificación:** {resultado}")
