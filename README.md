<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Master Seguros Pro - 130 Preguntas</title>
    <style>
        :root {
            --bg: #0f172a;
            --card: #1e293b;
            --accent: #0ea5e9;
            --gold: #f59e0b;
            --text: #f8fafc;
        }

        body {
            font-family: 'Segoe UI', system-ui, sans-serif;
            background-color: var(--bg);
            color: var(--text);
            margin: 0;
            padding-bottom: 40px;
        }

        .header {
            background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%);
            padding: 30px 20px;
            text-align: center;
            border-bottom: 2px solid var(--accent);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .player-controls {
            background: rgba(30, 41, 59, 0.8);
            backdrop-filter: blur(12px);
            margin: 15px;
            padding: 20px;
            border-radius: 20px;
            border: 1px solid rgba(255,255,255,0.1);
            box-shadow: 0 10px 25px rgba(0,0,0,0.3);
        }

        .btn-main {
            background: var(--accent);
            color: white;
            border: none;
            width: 100%;
            padding: 15px;
            border-radius: 12px;
            font-weight: bold;
            font-size: 1.1rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            transition: transform 0.2s;
        }

        .btn-main:active { transform: scale(0.98); }

        #contenedor { padding: 15px; }

        .card {
            background: var(--card);
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 15px;
            border-left: 4px solid transparent;
            transition: 0.3s;
            opacity: 0.4;
        }

        .card.active {
            opacity: 1;
            border-left-color: var(--gold);
            background: #2d3e50;
            transform: scale(1.02);
        }

        .q { font-weight: bold; color: var(--accent); margin-bottom: 8px; display: block; }
        .a { color: #10b981; font-size: 0.95rem; }

        select {
            width: 100%;
            margin-top: 15px;
            background: #0f172a;
            color: white;
            border: 1px solid var(--accent);
            padding: 10px;
            border-radius: 8px;
        }
    </style>
</head>
<body>

<div class="header">
    <h1 style="margin:0; font-size: 1.5rem;">🏆 Master Seguros Pro</h1>
    <div id="progress-text" style="font-size: 0.9rem; color: var(--gold); margin-top: 5px;">Listo para comenzar</div>
</div>

<div class="player-controls">
    <button class="btn-main" id="btnPlay" onclick="toggleMaraton()">
        <span id="icon">▶️</span> <span id="textBtn">INICIAR MARATÓN</span>
    </button>
    <select id="voiceSelect"></select>
</div>

<div id="contenedor"></div>

<audio id="silentAudio" loop>
    <source src="https://raw.githubusercontent.com/anars/blank-audio/master/10-seconds-of-silence.mp3" type="audio/mp3">
</audio>

<script>
    // Tu banco de preguntas completo
    const banco = [
        {p: "¿Cuál de las siguientes afirmaciones es CORRECTA sobre los nombramientos de beneficiarios?", r: "Al nombrar a un beneficiario irrevocable, el dueño de la póliza renuncia al derecho de cambiar de beneficiario."},
        {p: "¿Cuál de las siguientes anualidades paga generalmente los beneficios basándose en las unidades en lugar de los montos específicos en dólares?", r: "Una Anualidad Variable."},
        {p: "¿Cuál de las siguientes afirmaciones es CORRECTA sobre la disposición de Declaración Errónea de Edad?", r: "Requiere que se realice un ajuste de la cantidad de seguro si la edad del asegurado se declara de manera errada."},
        {p: "¿Cuál de los siguientes elementos de un contrato es necesario antes de emitir una póliza de vida?", r: "La firma de un prospecto en una solicitud."},
        {p: "El propósito de un Período de Gracia en una póliza de Muerte Accidental y Desmembramiento (AD&D) es ofrecerle al:", r: "dueño de la póliza tiempo adicional para pagar las primas que debe."},
        {p: "¿Cuál de las siguientes pólizas satisface MEJOR las necesidades de L (seguro de vida que se actualice con tasas de interés y permita contribuciones adicionales)?", r: "Vida de Término Renovable."},
        {p: "El tipo más común de póliza de salud en la que el titular de la póliza nombra a un beneficiario es:", r: "Muerte y Desmembramiento Accidental."},
        {p: "El objetivo de la disposición de Tiempo de Pago de las Reclamaciones es:", r: "evitar que la compañía de seguros se retrase en pagar las reclamaciones."},
        {p: "¿Cuál de los siguientes tipos de pólizas debe comprar un arquitecto (para cubrir salarios y alquiler si se discapacita)?", r: "Gastos Comerciales Generales."},
        {p: "Un plan de seguro de grupo para empleados en el que los empleados comparten los costos se considera un:", r: "plan contributivo."},
        {p: "Una anualidad de vida ofrece protección contra el riesgo de:", r: "vivir más tiempo de lo esperado."},
        {p: "¿Cuál de los siguientes tipos de pólizas de seguro de vida se usa generalmente para el seguro de vida de grupo?", r: "Término."},
        {p: "¿Cuál de los siguientes programas paga los beneficios que se basan en un porcentaje de la Cantidad de Seguro Primario (PIA)?", r: "Ingresos por Discapacidad del Seguro Social."},
        {p: "Si W se discapacita en un accidente antes de realizarse el examen médico obligatorio... ¿cuál de las siguientes acciones es más probable que tome la compañía de seguros?", r: "Negar la reclamación y reembolsar la prima pagada."},
        {p: "Para estar completamente asegurada bajo el Seguro Social, ¿cuántos trimestres como MÍNIMO DEBE haber trabajado la persona?", r: "40."},
        {p: "Una póliza de seguro médico a corto plazo se describe mejor como:", r: "cobertura provisional."},
        {p: "Bajo la Ley Uniforme de Muerte Simultánea (si no se sabe quién murió primero), los ingresos de la póliza se pagarán a:", r: "el patrimonio de G."},
        {p: "¿Por cuál de los siguientes se conoce un contrato que paga la mayoría de los beneficios... bajo la cláusula de Coordinación de Beneficios?", r: "Plan primario."},
        {p: "¿Cuál de las siguientes pólizas debe recomendar el agente (inversión con mayores ganancias que fianzas o CD, pero protegiendo capital)?", r: "Seguro de índice de acciones."},
        {p: "El empleado NO podrá calificar para beneficios... hasta después de un periodo de espera de cuántos meses?", r: "5."},
        {p: "Un beneficio Suplementario por Accidente de $500 en una póliza médica de grupo:", r: "pagaría 100% de los primeros cargos de $500 que resulten de un accidente."},
        {p: "Las afirmaciones substancialmente verdaderas realizadas por un solicitante se conocen como:", r: "representaciones."},
        {p: "¿Bajo cuál de los siguientes tipos de pólizas de seguro es el ingreso del asegurado el principal determinante para la cantidad de beneficios?", r: "Discapacidad."},
        {p: "La cantidad del pago de la reclamación del paciente será:", r: "de acuerdo con los términos de la póliza."},
        {p: "¿Cuál de las siguientes afirmaciones es CORRECTA sobre un plan de Vida a Término de grupo contributivo que provee $50,000 de cobertura?", r: "El empleador puede presentar una deducción de impuestos por su parte de los costos de las primas."},
        {p: "¿Cuál de las siguientes afirmaciones es CORRECTA sobre una póliza de Vida de Pago Limitado?", r: "Los pagos de las primas están limitados a un número específico de años."},
        {p: "¿Cuál es la cantidad MÁXIMA que recibiría por una reclamación aprobada (3 meses y medio de discapacidad, 30 días de eliminación, $500 mensuales)?", r: "$1,250."},
        {p: "¿Qué tipo de seguro HIPAA clasifica como seguro de Cuidado a Largo Plazo?", r: "Seguro de salud y accidentes."},
        {p: "¿Cuál de las siguientes disposiciones le permiten al asegurado cambiar la póliza a una póliza permanente sin mostrar evidencia de asegurabilidad?", r: "Disposición de Conversión."},
        {p: "El seguro de vida concedido para cubrir una necesidad por un periodo específico de tiempo a la prima más baja se llama:", r: "seguro de término nivelado."},
        {p: "¿Solo si el productor toma cuál de las siguientes acciones el mismo día comenzará la cobertura el día en que un asegurado firma una solicitud que no es de seguro médico?", r: "Cobra la prima inicial."},
        {p: "¿Tres años después (suicidio por sobredosis), la compañía de seguros pagará un MÁXIMO de?", r: "$20,000."},
        {p: "Un asegurado puede usar su póliza actual para satisfacer este requisito (garantía de préstamo) al:", r: "asignar la póliza al banco."},
        {p: "¿Cuál de las siguientes declaraciones es CORRECTA sobre que un tercero sea dueño de una póliza de seguro de vida?", r: "No se puede cambiar una vez que se emita la póliza."},
        {p: "¿Cuál de los siguientes tipos de recibo emitió el productor?", r: "Condicional."},
        {p: "¿Por cuántos días se ofrece un periodo de prueba sin compromiso para todas las pólizas de seguro de cuidado a largo plazo calificadas?", r: "30 días."},
        {p: "¿Cuál de las siguientes describe mejor una póliza de Vida Ajustable?", r: "Una póliza Ordinaria de Vida que provee beneficios por Muerte flexibles."},
        {p: "Entrega de la póliza se refiere a la entrega de:", r: "el contrato de seguro al solicitante."},
        {p: "¿Cuál de las siguientes afirmaciones sobre la responsabilidad de la compañía de seguros en esta situación (incendio intencional) es CORRECTA?", r: "La compañía no es responsable de más beneficios, y se debe rescindir el contrato de T."},
        {p: "Los beneficios de hospicio generalmente se incluyen en:", r: "Planes de cuidado administrado."},
        {p: "El beneficio por Enfermedad Crítica se le paga a:", r: "el asegurado."},
        {p: "¿Cuál de las siguientes acciones tomará una compañía de seguros si un productor presenta una solicitud incompleta?", r: "Le regresará la solicitud al productor."},
        {p: "El propósito de la Ley Patriótica de EE.UU. es detectar e impedir:", r: "terrorismo."},
        {p: "El tipo de anualidad comprada es una:", r: "Anualidad Mancomunada y de Sobreviviente de Prima Única Inmediata."},
        {p: "¿Cuál de las siguientes afirmaciones es CORRECTA sobre una cláusula adicional de Seguro Garantizado?", r: "Permite a un asegurado comprar seguro adicional a fechas específicas sin presentar prueba de asegurabilidad."},
        {p: "Este tipo de plan (dos socios para recuperar propiedad si uno muere) se llama:", r: "Acuerdo de Compraventa."},
        {p: "¿Bajo estas circunstancias, X califica para recibir los beneficios a partir de?", r: "1ro de junio."},
        {p: "Una cláusula adicional... que garantiza que la prima será pagada si el asegurado se discapacita se conoce como:", r: "Exoneración de prima."},
        {p: "En una póliza de Ingresos por Discapacidad, una declaración que excluye la cobertura por lesiones en la espalda se conoce como:", r: "una cláusula de enmienda por impedimento específico."},
        {p: "¿Cuál de las siguientes afirmaciones sobre una Cuenta de Retiro Individual (IRA) es CORRECTA?", r: "Una persona puede contribuir una parte de su ingreso devengado anualmente."},
        {p: "Si no se pagan beneficios... causadas por un acto de guerra, la compañía de seguros:", r: "excluirá la cobertura en las disposiciones del contrato."},
        {p: "Si un posible asegurado NO entrega la prima inicial junto con la solicitud, el productor debe:", r: "enviar la solicitud a la compañía de seguros sin la prima."},
        {p: "¿Cuál de las siguientes Opciones de Dividendos se define como seguro completamente pagado comprado con una prima única neta?", r: "Adiciones pagadas."},
        {p: "En esta situación (reclamación por alergias tras un año), ¿cuál de las siguientes acciones es más probable que tome la compañía de seguros?", r: "Pague la reclamación completa menos el pago del deducible y coaseguro."},
        {p: "Un contrato de seguros se considera aleatorio porque es:", r: "sujeto a la ocurrencia de un evento particular."},
        {p: "Medicare es provisto para:", r: "la mayoría de las personas de 65 años o más."},
        {p: "Para una póliza de la que es dueña un tercero, un solicitante que también sea el beneficiario primario designado debe:", r: "tener un interés asegurable en el asegurado prospectivo."},
        {p: "Debido a que las pólizas de seguro de salud se ofrecen como 'tómela o déjela', ¿se conocen como cuál tipo de contrato?", r: "Contratos de Adhesión."},
        {p: "¿Cuál de las siguientes afirmaciones sobre Medicaid es CORRECTA?", r: "Está financiado tanto por el gobierno federal como el estatal."},
        {p: "¿Cuál de las siguientes pólizas cubriría mejor estas necesidades (protección permanente a la prima anual más baja)?", r: "Vida de Pago Limitado."},
        {p: "A favor de cuál de las siguientes partes se interpreta cualquier redacción confusa en un contrato de adhesión?", r: "El asegurado."},
        {p: "Un programa de autorización de pre-hospitalización (pre-certificación) es un buen ejemplo de:", r: "cuidado administrado."},
        {p: "Un Perfil de Cobertura de una póliza Suplementaria de Medicare DEBE contener:", r: "las excepciones y limitaciones de la póliza."},
        {p: "El programa Medicare está financiado por:", r: "el gobierno federal solamente."},
        {p: "¿Quién retiene el derecho a nombrar a un beneficiario de un contrato de seguro de vida?", r: "El titular de la póliza."},
        {p: "¿Cuál de las siguientes afirmaciones es CORRECTA sobre el reemplazo de una póliza de seguro con otra?", r: "Está estrictamente regulado y requiere divulgación completa tanto del productor como de la compañía de seguros que reemplaza."},
        {p: "Un beneficiario contingente se describe MEJOR como una persona que es:", r: "nombrada por el asegurado para recibir los ingresos de la póliza si el beneficiario primario muere antes que el asegurado."},
        {p: "¿Cuál de las siguientes disposiciones para garantizar que las primas sean exoneradas si el titular se discapacita debe recomendar un productor... de Vida Juvenil?", r: "Cláusula de Pagador."},
        {p: "¿Cuál de las siguientes pólizas requiere que el productor esté inscrito con la NASD para poder venderla?", r: "Variable de Vida."},
        {p: "El propósito de la disposición de Acción Legal incluye todas las siguientes opciones EXCEPTO:", r: "proteger al productor."},
        {p: "Al entregar una póliza, el productor tiene que explicar que la disposición de periodo de prueba comienza cuando:", r: "el titular de la póliza reciba la póliza."},
        {p: "El Seguro de Vida Originado por un Inversionista (IOLI) es un acuerdo en el que la muerte del asegurado (beneficia al):", r: "asegurado."},
        {p: "La disposición de Tiempo de Pago de las Reclamaciones requiere que pague los beneficios por Discapacidad a una frecuencia no menor de:", r: "mensualmente."},
        {p: "Una póliza con valores en efectivo que fluctúan de acuerdo con el desempeño de las inversiones en acciones comunes se conoce como:", r: "Ordinaria de Vida Variable."},
        {p: "La disposición de una póliza de seguro que indique que la póliza no cubrirá ciertos riesgos se llama:", r: "una exclusión."},
        {p: "La recomendación adecuada (seguro de $250,000 con la prima más baja por los próximos 20 años) sería:", r: "término nivelado por 20 años."},
        {p: "¿Cuál de las siguientes pólizas ofrece beneficios por Muerte Accidental a los pasajeros de un avión?", r: "Cobertura total de accidentes."},
        {p: "Cuando un asegurado debe obtener el consentimiento del beneficiario para cambiar al beneficiario... se llama:", r: "irrevocable."},
        {p: "Si él muere el 21 de septiembre sin pagar la prima (vencía el 4 de sept), ¿cuál de las siguientes acciones tomaría la compañía?", r: "Pagar el beneficio por muerte menos la prima vencida."},
        {p: "¿Cuál de los siguientes artículos es MENOS probable que esté cubierto por una póliza Médica para Gastos Mayores?", r: "Anteojos correctivos."},
        {p: "¿Cuál de las siguientes pólizas le brindaría al participante la MEJOR cobertura para los gastos adicionales de hospitalización?", r: "Suplementaria de Medicare."},
        {p: "¿Cuál de las siguientes partes debe firmar o colocar sus iniciales en los cambios hechos a una solicitud de seguro?", r: "El solicitante."},
        {p: "¿Cuál de las siguientes afirmaciones sobre condiciones preexistentes es CORRECTA?", r: "Están cubiertas después de un periodo de tiempo específico indicado en la póliza."},
        {p: "¿Cuál de las siguientes pólizas debe comprar F (pago flexible, beneficio por muerte flexible y asume riesgo de inversión)?", r: "Vida Universal Variable."},
        {p: "Las afirmaciones hechas por el solicitante de una póliza de seguro de vida se conocen como:", r: "representaciones."},
        {p: "Con coaseguro 80/20 y deducible de $500, si el total de gastos cubiertos es de $1,500, el asegurado pagará un MÁXIMO de:", r: "$700."},
        {p: "Si a ella se le olvida pagar la prima y queda discapacitada el 31 de julio (vencía el 1 de julio), la compañía PROBABLEMENTE:", r: "pague la reclamación pero deduzca la prima no pagada."},
        {p: "Si N decide no pagar los intereses (del préstamo sobre la póliza) cuando es debido, la compañía automáticamente:", r: "añadirá el saldo del préstamo al monto adeudado de los intereses."},
        {p: "¿Cuál de las siguientes características de una póliza de seguro de vida hace posible un préstamo sobre la póliza?", r: "Disposición de valor en efectivo."},
        {p: "El propósito PRINCIPAL de la Oficina de Información Médica (MIB) es:", r: "registrar la información médica de los solicitantes para los seguros de vida y salud."},
        {p: "Si el asegurado decide no aceptar la póliza y la devuelve al productor el 24 de agosto (entregada el 15 de agosto), la compañía:", r: "devolverá la prima completa."},
        {p: "¿Qué ventaja ofrece al asegurado la característica de renovación de una póliza de término?", r: "Ofrece la misma cobertura sin aumento de primas."},
        {p: "La parte de un contrato de seguro que describe las condiciones bajo las que una compañía de seguros le paga los beneficios se conoce como:", r: "Página de declaración."},
        {p: "En seguros de vida, ¿en cuál de estos momentos debe existir un interés asegurable?", r: "Cuando se toma una solicitud."},
        {p: "Una póliza Ordinaria de Vida de Término a 65 tiene una prima anual que es:", r: "nivelada."},
        {p: "El objetivo PRINCIPAL de una cláusula de Coaseguro en una póliza médica para gastos mayores es:", r: "reducir la sobre-utilización de las coberturas."},
        {p: "¿Cuál de las siguientes pólizas puede proveerle a un empleado mayor de 65 años un ingreso mensual si no puede trabajar por enfermedad larga?", r: "Ingresos por Discapacidad."},
        {p: "¿Cuál describe el tratamiento de impuestos sobre los ingresos que se le da a los dividendos del titular de la póliza?", r: "Solamente se pagan impuestos sobre los intereses acumulados."},
        {p: "¿Cuál de las siguientes afirmaciones es CORRECTA sobre una disposición de Discapacidad Recurrente?", r: "Aplica a un segundo periodo de discapacidad por la misma enfermedad dentro de un periodo de tiempo especificado."},
        {p: "Las pólizas individuales y las de grupos pequeños que se venden en el Mercado de Seguros Médicos:", r: "no estarán sujetas a las exclusiones de condición preexistente."},
        {p: "¿En qué momento DEBE comenzar la cobertura de seguro de salud para los recién nacidos que tienen anormalidades congénitas?", r: "Al nacer."},
        {p: "Una compañía de seguros de acciones es una compañía cuyos dueños son los:", r: "accionistas."},
        {p: "Para un plan médico que califique bajo la ACA, los Beneficios de Salud Esenciales se tienen que incluir:", r: "para todas las pólizas que se vendan en el Mercado de Seguros Médicos."},
        {p: "De qué forma se proveen los beneficios que paga una HMO?", r: "Pre-pago."},
        {p: "¿Cuál de las siguientes acciones por parte de una compañía de seguros se considera una práctica de liquidación injusta?", r: "Negarle la reclamación a un asegurado sin indicar la razón de la negación bajo la póliza."},
        {p: "Una organización que solicita seguro solo entre sus miembros se conoce como:", r: "una sociedad de beneficios fraternales."},
        {p: "Se debe incluir todo lo siguiente en un Resumen de Beneficios y Cobertura EXCEPTO:", r: "prima."},
        {p: "Bajo las leyes de seguros de Texas, el término 'hacer negocios' incluye:", r: "cobrar las primas."},
        {p: "¿Cuál de las siguientes acciones de un agente se considera como la práctica de rebajas?", r: "Compartir comisiones con un asegurado que compró una cobertura recientemente."},
        {p: "¿Quién puede comprar un plan a través del Mercado de Seguros Médicos?", r: "Cualquier residente legal excepto aquellos que estén en la cárcel."},
        {p: "¿El formulario de Evidencia de Cobertura de una HMO debe ser aprobado por?", r: "El Comisionado de Seguros."},
        {p: "¿Por cuántos años MÍNIMO debe un individuo con licencia llevar registro de las transacciones de seguro?", r: "Cinco."},
        {p: "Bajo el código de seguros de Texas, las compañías deben pagar beneficios de muerte por suicidio si la póliza ha estado vigente por un MÍNIMO de:", r: "dos años."},
        {p: "¿Cuál disposición evitaría que una póliza caduque en un mes a partir de la fecha de pago de la prima?", r: "Período de gracia."},
        {p: "Si se determina que un agente participó en prácticas injustas, el Comisionado de Seguros puede:", r: "ordenar que el agente deje las prácticas."},
        {p: "Cuando se reemplaza una póliza de vida, ¿cuál de los siguientes formularios le DEBE proveer el agente al solicitante?", r: "Aviso relacionado con el reemplazo del seguro de vida."},
        {p: "¿Cuál es el MÍNIMO de horas de educación continuada que le exige el Departamento de Seguros de Texas al agente?", r: "24."},
        {p: "Para que una compañía de seguros pueda llevar a cabo negocios en Texas, ¿cuál de las siguientes DEBE ser cierta?", r: "Debe tener un certificado de autoridad emitido por Texas."},
        {p: "Los agentes que ofrezcan una póliza Suplementaria de Medicare DEBEN presentarse como agentes de:", r: "La compañía que representan."},
        {p: "¿Cuál de los siguientes DEBEN notificarle los agentes de seguro... al Departamento de Seguros de Texas?", r: "Cualquier acción administrativa que tome en contra del agente un regulador financiero o de seguros en cualquier estado."},
        {p: "Cuando una persona está inscrita en una Organización para el Mantenimiento de la Salud (HMO), puede utilizar un proveedor fuera del área:", r: "durante una emergencia."},
        {p: "Un propósito de la regla de reemplazo de seguro de vida y anualidad es proteger:", r: "los intereses del titular de la póliza."},
        {p: "Si un asegurado bajo un contrato de vida grupal no continúa trabajando y se muere durante el periodo de conversión, la compañía de seguros:", r: "pagará la reclamación completa."},
        {p: "¿Cuál de los siguientes NO es un ejemplo de fraude de seguros?", r: "Iniciar cambios en una solicitud de póliza."},
        {p: "Una compañía de seguros extranjera es una que no está incorporada:", r: "bajo la ley de Texas."},
        {p: "La disposición que trata sobre Declaración de Edad Incorrecta requiere que si la edad del asegurado está errada, entonces cualquier cantidad pagadera será:", r: "una cantidad que las primas pagadas hubieran comprado por la edad o edades correctas."},
        {p: "¿Por cuántos años es generalmente válida la renovación de una licencia de seguros?", r: "Dos años."},
        {p: "Un agente publica en las redes sociales que garantiza dividendos en una póliza de Vida Universal. El agente es culpable de:", r: "publicidad falsa."},
        {p: "Bajo la Ley de Cuidado de Salud a Bajo Precio, la cantidad máxima a cargo del asegurado incluye:", r: "Los copagos, deducibles y coaseguro."}
    ];

    const synth = window.speechSynthesis;
    const silentAudio = document.getElementById('silentAudio');
    const contenedor = document.getElementById('contenedor');
    let currentIdx = 0;
    let isPlaying = false;
    let currentUtterance = null;

    // Renderizar tarjetas
    function renderizar() {
        contenedor.innerHTML = banco.map((item, i) => `
            <div class="card" id="card-${i}" onclick="saltarA(${i})">
                <span class="q">${i + 1}. ${item.p}</span>
                <div class="a">${item.r}</div>
            </div>
        `).join('');
    }

    function saltarA(index) {
        currentIdx = index;
        if(isPlaying) {
            synth.cancel();
            hablar();
        } else {
            actualizarUI();
        }
    }

    function cargarVoces() {
        const voices = synth.getVoices();
        const select = document.getElementById('voiceSelect');
        const spanishVoices = voices.filter(v => v.lang.includes('es'));
        
        select.innerHTML = spanishVoices
            .map(v => `<option value="${v.name}">${v.name} (${v.lang})</option>`).join('');
    }
    
    window.speechSynthesis.onvoiceschanged = cargarVoces;

    function toggleMaraton() {
        if (isPlaying) {
            pausar();
        } else {
            reproducir();
        }
    }

    function reproducir() {
        isPlaying = true;
        silentAudio.play();
        document.getElementById('textBtn').innerText = "PAUSAR MARATÓN";
        document.getElementById('icon').innerText = "⏸️";
        hablar();
        actualizarMediaSession();
    }

    function pausar() {
        isPlaying = false;
        synth.cancel();
        silentAudio.pause();
        document.getElementById('textBtn').innerText = "REANUDAR";
        document.getElementById('icon').innerText = "▶️";
        actualizarMediaSession();
    }

    function actualizarUI() {
        document.querySelectorAll('.card').forEach(c => c.classList.remove('active'));
        const cardActiva = document.getElementById(`card-${currentIdx}`);
        if (cardActiva) {
            cardActiva.classList.add('active');
            cardActiva.scrollIntoView({ behavior: 'smooth', block: 'center' });
        }
        document.getElementById('progress-text').innerText = `Pregunta ${currentIdx + 1} de ${banco.length}`;
    }

    function hablar() {
        if (!isPlaying || currentIdx >= banco.length) return;

        actualizarUI();
        
        const texto = `Pregunta ${currentIdx + 1}: ${banco[currentIdx].p}. Respuesta: ${banco[currentIdx].r}`;
        currentUtterance = new SpeechSynthesisUtterance(texto);
        
        const voices = synth.getVoices();
        const selectedVoice = voices.find(v => v.name === document.getElementById('voiceSelect').value);
        if (selectedVoice) currentUtterance.voice = selectedVoice;
        
        currentUtterance.rate = 0.95;
        currentUtterance.pitch = 1;

        currentUtterance.onend = () => {
            if (isPlaying) {
                currentIdx++;
                if (currentIdx < banco.length) {
                    setTimeout(hablar, 1200);
                } else {
                    pausar();
                }
            }
        };

        actualizarMediaSession();
        synth.speak(currentUtterance);
    }

    // --- CONFIGURACIÓN DE AUDÍFONOS Y PANTALLA DE BLOQUEO ---
    function actualizarMediaSession() {
        if ('mediaSession' in navigator) {
            navigator.mediaSession.metadata = new MediaMetadata({
                title: `Pregunta ${currentIdx + 1}`,
                artist: 'Master Seguros Pro',
                album: 'Estudio para Licencia de Seguros',
                artwork: [
                    { src: 'https://cdn-icons-png.flaticon.com/512/3061/3061324.png', sizes: '512x512', type: 'image/png' }
                ]
            });

            navigator.mediaSession.playbackState = isPlaying ? "playing" : "paused";

            // Control: Siguiente Pregunta
            navigator.mediaSession.setActionHandler('nexttrack', () => {
                if (currentIdx < banco.length - 1) {
                    currentIdx++;
                    synth.cancel();
                    if(isPlaying) hablar(); else actualizarUI();
                }
            });

            // Control: Pregunta Anterior
            navigator.mediaSession.setActionHandler('previoustrack', () => {
                if (currentIdx > 0) {
                    currentIdx--;
                    synth.cancel();
                    if(isPlaying) hablar(); else actualizarUI();
                }
            });

            // Control: Play/Pause desde audífonos
            navigator.mediaSession.setActionHandler('play', () => reproducir());
            navigator.mediaSession.setActionHandler('pause', () => pausar());
        }
    }

    // Inicialización
    renderizar();
    setTimeout(cargarVoces, 500);
</script>

</body>
</html>