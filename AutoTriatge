import streamlit as st

# Configuración de la página web
st.set_page_config(page_title="Autotriatge d'Urgències", page_icon="🏥", layout="centered")

# Inicializar variables de estado para controlar la página actual del formulario
if "pagina" not in st.session_state:
    st.session_state.pagina = 1
if "nombre" not in st.session_state:
    st.session_state.nombre = ""
if "edad" not in st.session_state:
    st.session_state.edad = 18
if "motivo" not in st.session_state:
    st.session_state.motivo = ""
if "gravedad" not in st.session_state:
    st.session_state.gravedad = 0

st.title("🏥 BENVINGUT A L'AUTOTRIATGE D'URGÈNCIES")
st.write("---")

# ==========================================
# PÀGINA 1: DADES BÀSIQUES
# ==========================================
if st.session_state.pagina == 1:
    st.subheader("Si us plau, introdueix les teves dades:")
    nom_input = st.text_input("Nom del pacient:", value=st.session_state.nombre)
    edat_input = st.number_input("Edat:", min_value=0, max_value=120, value=st.session_state.edad)
    
    if st.button("Següent ➡️", type="primary"):
        if nom_input.strip() == "":
            st.error("Si us plau, introdueix un nom vàlid.")
        else:
            st.session_state.nombre = nom_input
            st.session_state.edad = edat_input
            st.session_state.pagina = 2
            st.rerun()

# ==========================================
# PÀGINA 2: SELECCIÓ DEL MOTIU
# ==========================================
elif st.session_state.pagina == 2:
    st.subheader(f"Hola {st.session_state.nombre}. Quin és el motiu principal de la teva visita?")
    
    if st.button("🫁 Dolor fort al pit o falta d'aire", use_container_width=True):
        st.session_state.motivo = "pecho"
        st.session_state.pagina = 3
        st.rerun()
    if st.button("💥 He tingut un accident / cop / ferida", use_container_width=True):
        st.session_state.motivo = "golpe"
        st.session_state.pagina = 3
        st.rerun()
    if st.button("🤢 Dolor d'estómac o de panxa sever", use_container_width=True):
        st.session_state.motivo = "tripa"
        st.session_state.pagina = 3
        st.rerun()
    if st.button("🤒 Tinc febre o malestar general", use_container_width=True):
        st.session_state.motivo = "fiebre"
        st.session_state.pagina = 3
        st.rerun()

# ==========================================
# PÀGINA 3: PREGUNTES ESPECÍFIQUES
# ==========================================
elif st.session_state.pagina == 3:
    st.subheader("Explica'ns una mica més sobre el teu símptoma:")
    
    preguntas = {
        "pecho": [
            "Em costa molt respirar o sento opressió al pit",
            "És un dolor punxant quan tusso",
            "És un dolor lleu que va i ve",
            "Tinc palpitacions però respiro bé"
        ],
        "golpe": [
            "Hi ha un os visible o un sagnat que no para",
            "No puc moure la zona i s'està inflant molt",
            "Puc moure-ho però em fa mal en recolzar-me",
            "És només una rascada o un cop lleu"
        ],
        "tripa": [
            "El dolor és insuportable i tinc vòmits amb sang",
            "És un dolor molt agut que ha començat de cop",
            "És un dolor sord constant",
            "Tinc molèsties lleus o gasos"
        ],
        "fiebre": [
            "Tinc més de 40ºC i em costa mantenir la consciència",
            "Tinc tremolors, calfreds i més de 38.5ºC",
            "Tinc dècimes (37.5ºC - 38ºC) i mal de cap",
            "No tinc febre però em trobo destemplat/da"
        ]
    }
    
    opcions_simptoma = preguntas[st.session_state.motivo]
    
    # El pacient tria una de les 4 opcions clares
    seleccio = st.radio("Selecciona la frase que millor descriu el que sents:", opcions_simptoma)
    
    if st.button("Següent ➡️", type="primary"):
        # Guardem la gravetat segons l'índex seleccionat (0 més greu, 3 menys greu)
        st.session_state.gravedad = opcions_simptoma.index(seleccio)
        st.session_state.pagina = 4
        st.rerun()

# ==========================================
# PÀGINA 4: ESCALA DE DOLOR
# ==========================================
elif st.session_state.pagina == 4:
    st.subheader("Per últim, assenyala el teu nivell de dolor:")
    st.write("(Esquerra menys dolor, Dreta màxim dolor possible)")
    
    dolor = st.slider("Escala de dolor:", min_value=1, max_value=10, value=5)
    
    if st.button("Veure Resultat 🏁", type="primary"):
        st.session_state.dolor = dolor
        st.session_state.pagina = 5
        st.rerun()

# ==========================================
# PÀGINA 5: RESULTAT FINAL
# ==========================================
elif st.session_state.pagina == 5:
    st.subheader("🏥 RESUM DEL TEU AUTOTRIATGE")
    
    # Lògica de decisió del sistema de triatge
    color = "🟢 VERD (Baixa prioritat)"
    instrucciones = "Si us plau, agafa lloc a la sala d'espera general. Un infermer/a et cridarà."
    estilo_alerta = "success"
    
    motivo = st.session_state.motivo
    gravedad = st.session_state.gravedad
    dolor = st.session_state.dolor
    
    if motivo == "pecho" and gravedad == 0 or gravedad == 0 and dolor == 10:
        color = "🔴 VERMELL (Emergència Immediata)"
        instrucciones = "¡AVISA EL PERSONAL DE L'ENTRADA IMMEDIATAMENT! Passa directe al box vital."
        estilo_alerta = "error"
    elif gravedad == 0 or gravedad == 1 and dolor >= 8:
        color = "🟠 TARONJA (Molt Urgent)"
        instrucciones = "Passa a la zona d'atenció prioritària. Temps estimat: menys de 10 minuts."
        estilo_alerta = "warning"
    elif gravedad == 1 or gravedad == 2 and dolor >= 6:
        color = "🟡 GROC (Urgent)"
        instrucciones = "Espera a la sala d'Urgències. Temps estimat d'atenció: menys de 60 minuts."
        estilo_alerta = "warning"

    # Mostrar la targeta visual final al pacient
    st.info(f"**Pacient:** {st.session_state.nombre} | **Edat:** {st.session_state.edad} anys")
    
    if estilo_alerta == "success":
        st.success(f"**RESULTAT:** {color}\n\n**Què has de fer?:** {instrucciones}")
    elif estilo_alerta == "warning":
        st.warning(f"**RESULTAT:** {color}\n\n**Què has de fer?:** {instrucciones}")
    else:
        st.error(f"**RESULTAT:** {color}\n\n**Què has de fer?:** {instrucciones}")
        
    if st.button("🔄 Registrar un altre pacient"):
        # Reiniciar variables
        st.session_state.pagina = 1
        st.session_state.nombre = ""
        st.session_state.edad = 18
        st.session_state.motivo = ""
        st.session_state.gravedad = 0
        st.rerun()
