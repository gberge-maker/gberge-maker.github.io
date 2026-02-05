<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Retórica Ultra - PAU 2026</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; background-color: #020617; margin: 0; color: white; overflow-x: hidden; }
        .perspective-1000 { perspective: 1000px; }
        .backface-hidden { backface-visibility: hidden; }
        .transform-style-3d { transform-style: preserve-3d; }
        .rotate-y-180 { transform: rotateY(180deg); }
        .fade-in { animation: fadeIn 0.5s ease-out forwards; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #020617; }
        ::-webkit-scrollbar-thumb { background: #1e293b; border-radius: 10px; }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect, useMemo } = React;

        // Definición de iconos para uso interno
        const Icons = {
            Sparkles: () => <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="m12 3-1.912 5.813a2 2 0 0 1-1.275 1.275L3 12l5.813 1.912a2 2 0 0 1 1.275 1.275L12 21l1.912-5.813a2 2 0 0 1 1.275-1.275L21 12l-5.813-1.912a2 2 0 0 1-1.275-1.275L12 3Z"/><path d="M5 3v4M19 17v4M3 5h4M17 19h4"/></svg>,
            Zap: () => <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M4 14.71 14 3l-3 9h9l-10 11.71 3-9H4Z"/></svg>,
            Repeat: () => <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="m17 2 4 4-4 4M3 11v-1a4 4 0 0 1 4-4h14m-14 16-4-4 4-4m14 9v1a4 4 0 0 1-4 4H3"/></svg>,
            Book: () => <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M4 19.5v-15A2.5 2.5 0 0 1 6.5 2H20v20H6.5a2.5 2.5 0 0 1 0-5H20"/></svg>,
            Search: () => <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.3-4.3"/></svg>,
            Trophy: ({size = 64}) => <svg xmlns="http://www.w3.org/2000/svg" width={size} height={size} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M6 9H4.5a2.5 2.5 0 0 1 0-5H6M18 9h1.5a2.5 2.5 0 0 0 0-5H18M4 22h16M10 14.66V17c0 .55-.47.98-.97 1.21C7.85 18.75 7 20.24 7 22M14 14.66V17c0 .55.47.98.97 1.21C16.15 18.75 17 20.24 17 22M18 2H6v7a6 6 0 0 0 12 0V2Z"/></svg>,
            X: () => <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M18 6 6 18M6 6 12 12"/></svg>,
            Graduation: () => <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M22 10v6M2 10l10-5 10 5-10 5zM6 12v5c0 2 2 3 6 3s6-1 6-3v-5"/></svg>,
            CreativeCommons: () => <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><circle cx="12" cy="12" r="10"/><circle cx="12" cy="12" r="6"/><path d="M14 9a2.5 2.5 0 0 0-4 2v2a2.5 2.5 0 0 0 4 2"/></svg>,
            Keyboard: () => <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><rect width="20" height="16" x="2" y="4" rx="2"/><path d="M6 8h.01M10 8h.01M14 8h.01M18 8h.01M6 12h.01M18 12h.01M10 12h.01M14 12h.01M8 16h8"/></svg>
        };

        const App = () => {
            const [activeTab, setActiveTab] = useState('sintaxis');
            const [searchTerm, setSearchTerm] = useState('');
            const [quizMode, setQuizMode] = useState(false);
            const [quizType, setQuizType] = useState('choice'); 
            const [score, setScore] = useState(0);
            const [currentQuestion, setCurrentQuestion] = useState(0);
            const [showResult, setShowResult] = useState(null);
            const [quizFinished, setQuizFinished] = useState(false);
            const [questions, setQuestions] = useState([]);
            const [matchingPairs, setMatchingPairs] = useState([]);
            const [selectedExample, setSelectedExample] = useState(null);
            const [selectedTerm, setSelectedTerm] = useState(null);
            const [flashcardIndex, setFlashcardIndex] = useState(0);
            const [isFlipped, setIsFlipped] = useState(false);

            // Normalización segura para evitar errores en el buscador
            const normalizeText = (text) => {
                if (!text) return "";
                return String(text).normalize("NFD").replace(/[\u0300-\u036f]/g, "").toLowerCase();
            };

            // BASE DE DATOS EXACTA CON LAS 33 FIGURAS
            const CATEGORIAS_DATA = useMemo(() => ({
                sintaxis: [
                    {
                        grupo: "Nivel Fónico y Ritmo",
                        color: "bg-blue-500",
                        items: [
                            { nombre: "Aliteración", def: "Repetición de sonidos similares para evocar una sensación.", ej: "En el silencio solo se escuchaba \nun susurro de abejas que sonaba.", autor: "Garcilaso de la Vega", maestro: "Busca imitar un sonido real mediante la fonética.", ejAlt: "A las aladas almas de las rosas.", autorAlt: "Miguel Hernández", iconName: "Zap" },
                            { nombre: "Anadiplosis", def: "Repetición de la última palabra de un verso al principio del siguiente.", ej: "Nadie establece leyes en la guerra; \nla guerra que yo hago es de paciencia.", autor: "Francisco de Quevedo", maestro: "Encadena ideas creando un ritmo de cascada.", ejAlt: "La plaza tiene una torre,\nla torre tiene un balcón.", autorAlt: "Antonio Machado", iconName: "Repeat" },
                            { nombre: "Anáfora", def: "Repetición de palabras al comienzo de varios versos consecutivos.", ej: "Temprano levantó la muerte el vuelo,\ntemprano madrugó la madrugada.", autor: "Miguel Hernández", maestro: "Crea un ritmo insistente y enfatiza el concepto.", ejAlt: "Pena con pena y pena desayuno,\npena es mi paz y pena mi batalla.", autorAlt: "Miguel Hernández", iconName: "Repeat" },
                            { nombre: "Paralelismo", def: "Repetición de una misma estructura sintáctica en varios versos.", ej: "La tierra más que el cuerpo se resiente; \nla sangre más que el alma se dilata.", autor: "Garcilaso de la Vega", maestro: "Busca la simetría gramatical absoluta entre líneas.", ejAlt: "Los suspiros son aire y van al aire.\nLas lágrimas son agua y van al mar.", autorAlt: "Gustavo Adolfo Bécquer", iconName: "Repeat" },
                            { nombre: "Políptoton", def: "Uso de una misma palabra con diferentes accidentes gramaticales.", ej: "No me mires, que miran \nque nos miramos.", autor: "Tradicional", maestro: "Juego de ingenio basado en la flexión de la palabra.", ejAlt: "¿Cómo quieres que te quiera,\nsi el que quiero no me quiere?", autorAlt: "Anónimo", iconName: "Repeat" },
                            { nombre: "Epanadiplosis", def: "Repetición de la misma palabra al principio y al final de un mismo verso.", ej: "Verde que te quiero verde.", autor: "Federico García Lorca", maestro: "Crea un círculo cerrado de pensamiento.", ejAlt: "Silencio de la noche, doloroso silencio.", autorAlt: "Rubén Darío", iconName: "Repeat" },
                            { nombre: "Epífora", def: "Repetición de una o varias palabras al final de versos consecutivos.", ej: "De padres gitanos, gitanos,\nde abuelos gitanos, gitanos.", autor: "Cervantes", maestro: "Cierra con fuerza cada unidad poética.", ejAlt: "No decía palabras,\nacercaba tan solo un cuerpo como un cuerpo.", autorAlt: "Luis Cernuda", iconName: "Repeat" }
                        ]
                    },
                    {
                        grupo: "Orden y Estructura",
                        color: "bg-indigo-600",
                        items: [
                            { nombre: "Hipérbaton", def: "Alteración del orden lógico gramatical de las palabras.", ej: "Volverán las oscuras golondrinas \nen tu balcón sus nidos a colgar.", autor: "Bécquer", maestro: "Imita la sintaxis latina para elevar el tono.", ejAlt: "Del salón en el ángulo oscuro...", autorAlt: "Bécquer", iconName: "Repeat" },
                            { nombre: "Quiasmo", def: "Ordenación cruzada de elementos: A-B / B-A.", ej: "Ni son todos los que están, \nni están todos los que son.", autor: "Refranero", maestro: "Visualízalo como una 'X' de conceptos.", ejAlt: "¿Siempre se ha de sentir lo que se dice?\n¿Nunca se ha de decir lo que se siente?", autorAlt: "Quevedo", iconName: "Repeat" },
                            { nombre: "Paronomasia", def: "Uso de palabras de sonido similar pero distinto significado.", ej: "Vendado que me has vendido.", autor: "Góngora", maestro: "Crea un juego fonético sorprendente.", ejAlt: "Entre casar y cansar.", autorAlt: "Tradicional", iconName: "Repeat" },
                            { nombre: "Enumeración", def: "Acumulación de elementos para describir un concepto.", ej: "En tierra, en humo, en polvo, en sombra, en nada.", autor: "Góngora", maestro: "Da una visión de conjunto detallada.", ejAlt: "Goza cuello, cabello, labio y frente.", autorAlt: "Góngora", iconName: "Repeat" }
                        ]
                    },
                    {
                        grupo: "Adición u Omisión",
                        color: "bg-cyan-500",
                        items: [
                            { nombre: "Asíndeton", def: "Omisión de nexos o conjunciones para dar rapidez.", ej: "Acude, corre, vuela,\ntraspasa la alta sierra, ocupa el llano.", autor: "Fray Luis de León", maestro: "Elimina las pausas para acelerar el ritmo.", ejAlt: "Llegué, vi, vencí.", autorAlt: "Julio César", iconName: "Repeat" },
                            { nombre: "Polisíndeton", def: "Repetición innecesaria de conjunciones para dar lentitud.", ej: "Y ríe, y llora, y aborrece, y ama,\ny guarda un rastro del dolor pasado.", autor: "Bécquer", maestro: "Obliga al lector a detenerse en cada palabra.", ejAlt: "Soy un fue y un será y un es cansado.", autorAlt: "Quevedo", iconName: "Zap" },
                            { nombre: "Elipsis", def: "Omisión de un elemento que se sobreentiende.", ej: "Lo bueno, si breve, dos veces bueno.", autor: "Baltasar Gracián", maestro: "Aporta densidad y brevedad al pensamiento.", ejAlt: "Yo llevaba las flores; ella, las cintas.", autorAlt: "Anónimo", iconName: "Repeat" },
                            { nombre: "Zeugma", def: "Un elemento omitido se sobrentiende por aparecer antes.", ej: "La niña es rubia; el niño, moreno.", autor: "Anónimo", maestro: "Un solo verbo sirve para varios enunciados.", ejAlt: "Frida pintaba cuadros; Diego, murales.", autorAlt: "Anónimo", iconName: "Repeat" },
                            { nombre: "Pleonasmo", def: "Uso de palabras innecesarias que ya están implícitas.", ej: "De los sus ojos tan fuertemente llorando.", autor: "Cantar de Mio Cid", maestro: "Refuerza la expresividad mediante la redundancia.", ejAlt: "Lo vi con mis propios ojos.", autorAlt: "Anónimo", iconName: "Book" }
                        ]
                    }
                ],
                semantica: [
                    {
                        grupo: "Relaciones y Tropos",
                        color: "bg-rose-500",
                        items: [
                            { nombre: "Metáfora", def: "Identificación de un término real con uno imaginario.", ej: "Las perlas de tu boca.", autor: "Anónimo", maestro: "Es una identidad total, no usa 'como'.", ejAlt: "Su luna de pergamino\nPreciosa tocando viene.", autorAlt: "Lorca", iconName: "Sparkles" },
                            { nombre: "Metonimia", def: "Sustitución basada en relaciones de proximidad.", ej: "Se lee a Cervantes.", autor: "Anónimo", maestro: "Sustituye autor por obra o lugar por producto.", ejAlt: "Se tomó una copa.", autorAlt: "Anónimo", iconName: "Repeat" },
                            { nombre: "Símbolo", def: "Objeto que representa una idea abstracta o sentimiento.", ej: "Nuestras vidas son los ríos / que van a dar en la mar.", autor: "Jorge Manrique", maestro: "Imagen física que encierra un concepto espiritual.", ejAlt: "Caminante, son tus huellas el camino.", autorAlt: "Machado", iconName: "Sparkles" },
                            { nombre: "Símil o comparación", def: "Comparación explícita mediante nexos (como, cual).", ej: "Tus ojos son como luceros.", autor: "Anónimo", maestro: "Mantiene los dos términos vivos mediante un nexo.", ejAlt: "Sus manos eran como naves.", autorAlt: "Luis Cernuda", iconName: "Sparkles" },
                            { nombre: "Sinestesia", def: "Atribución de una sensación a un sentido distinto.", ej: "Escucho con los ojos a los muertos.", autor: "Quevedo", maestro: "Mezcla sensaciones (color chillón, voz dulce).", ejAlt: "Voz aterciopelada.", autorAlt: "Anónimo", iconName: "Zap" },
                            { nombre: "Hipálage", def: "Atribuir un adjetivo a un sustantivo que no le corresponde.", ej: "El oro temeroso de la tumba.", autor: "Quevedo", maestro: "El adjetivo se desplaza de su dueño lógico.", ejAlt: "El público llenaba las gradas ruidosas.", autorAlt: "Borges", iconName: "Book" }
                        ]
                    },
                    {
                        grupo: "Figuras de Pensamiento",
                        color: "bg-amber-500",
                        items: [
                            { nombre: "Hipérbole", def: "Exageración de la realidad para impactar.", ej: "Érase un hombre a una nariz pegado.", autor: "Quevedo", maestro: "Dibuja una realidad imposible por exceso.", ejAlt: "Tanto dolor se agrupa en mi costado.", autorAlt: "Hernández", iconName: "Sparkles" },
                            { nombre: "Personificación", def: "Atribuir rasgos humanos a objetos o animales.", ej: "La lluvia lloraba sobre el cristal.", autor: "Anónimo", maestro: "Dota de alma y voluntad a lo inanimado.", ejAlt: "La ciudad era un animal que respiraba.", autorAlt: "Anónimo", iconName: "Sparkles" },
                            { nombre: "Antítesis", def: "Oposición de dos ideas empleando antónimos.", ej: "Es tan corto el amor y tan largo el olvido.", autor: "Neruda", maestro: "Contrapone términos para resaltar uno de ellos.", ejAlt: "Yo velo mientras tú duermes.", autorAlt: "Anónimo", iconName: "Repeat" },
                            { nombre: "Paradoja", def: "Unión de ideas contradictorias con sentido profundo.", ej: "Vivo sin vivir en mí.", autor: "Santa Teresa", maestro: "Parece un error lógico, pero encierra una verdad.", ejAlt: "Solo sé que no sé nada.", autorAlt: "Sócrates", iconName: "Zap" },
                            { nombre: "Oxímoron", def: "Contradicción entre dos términos contiguos.", ej: "Hielo abrasador.", autor: "Quevedo", maestro: "Dos palabras juntas que se niegan entre sí.", ejAlt: "La música callada.", autorAlt: "San Juan de la Cruz", iconName: "Repeat" },
                            { nombre: "Epíteto", def: "Adjetivo que expresa una cualidad intrínseca.", ej: "Blanca nieve.", autor: "Anónimo", maestro: "Enfatiza lo obvio para embellecer el verso.", ejAlt: "Oscura noche.", autorAlt: "San Juan de la Cruz", iconName: "Sparkles" },
                            { nombre: "Apóstrofe", def: "Invocación apasionada a algo o alguien ausente.", ej: "¡Oh noche que guiaste!", autor: "San Juan de la Cruz", maestro: "El poeta se dirige directamente a su interlocutor.", ejAlt: "Para y óyeme, ¡oh Sol!", autorAlt: "Espronceda", iconName: "Repeat" },
                            { nombre: "Eufemismo", def: "Expresión suave para sustituir algo duro o tabú.", ej: "Pasó a mejor vida.", autor: "Tradicional", maestro: "Maquillaje del lenguaje para evitar la crudeza.", ejAlt: "Conflicto armado.", autorAlt: "Anónimo", iconName: "Book" },
                            { nombre: "Interrogación retórica", def: "Pregunta que no busca respuesta, sino reflexión.", ej: "¿Qué se hicieron las damas, sus tocados?", autor: "Jorge Manrique", maestro: "Es una afirmación en forma de pregunta.", ejAlt: "¿Será la muerte el sueño?", autorAlt: "Borges", iconName: "Keyboard" },
                            { nombre: "Ironía", def: "Dar a entender lo contrario de lo que se dice.", ej: "¡Qué buena suerte tengo!", autor: "Tradicional", maestro: "Requiere que el lector entienda el sarcasmo.", ejAlt: "Salió con tanta honra de la cárcel...", autorAlt: "Quevedo", iconName: "Keyboard" },
                            { nombre: "Lítote o atenuación", def: "Negar lo contrario de lo que se desea afirmar.", ej: "No es poco inteligente.", autor: "Tradicional", maestro: "Afirma algo negando su opuesto por prudencia.", ejAlt: "Vio que no estaba lejos.", autorAlt: "Cervantes", iconName: "Repeat" }
                        ]
                    }
                ]
            }), []);

            // Figuras y Banco Maestro
            const figurasOficiales = useMemo(() => [...CATEGORIAS_DATA.sintaxis, ...CATEGORIAS_DATA.semantica].flatMap(g => g.items.map(i => i.nombre)), [CATEGORIAS_DATA]);
            const masterBank = useMemo(() => [...CATEGORIAS_DATA.sintaxis, ...CATEGORIAS_DATA.semantica].flatMap(g => g.items).map(item => ({ q: item.ej, correct: item.nombre, autor: item.autor })), [CATEGORIAS_DATA]);

            const startQuiz = (type) => {
                const shuffled = [...masterBank].sort(() => 0.5 - Math.random()).slice(0, 10);
                if (type === 'choice') {
                    setQuestions(shuffled.map(item => ({ 
                        ...item, 
                        options: [...figurasOficiales.filter(f => f !== item.correct).sort(() => 0.5 - Math.random()).slice(0, 2), item.correct].sort() 
                    })));
                } else if (type === 'matching') {
                    setMatchingPairs(shuffled.slice(0, 5));
                } else if (type === 'flashcards') {
                    setQuestions(shuffled);
                    setFlashcardIndex(0);
                    setIsFlipped(false);
                }
                setQuizType(type);
                setQuizMode(true);
                setScore(0);
                setCurrentQuestion(0);
                setQuizFinished(false);
                setShowResult(null);
            };

            useEffect(() => {
                if (selectedExample && selectedTerm) {
                    if (selectedExample.correct === selectedTerm.correct) {
                        setScore(s => s + 1);
                        const q = selectedExample.q;
                        setTimeout(() => { setMatchingPairs(p => p.filter(x => x.q !== q)); setSelectedExample(null); setSelectedTerm(null); }, 300);
                    } else {
                        setShowResult('incorrect');
                        setTimeout(() => { setShowResult(null); setSelectedExample(null); setSelectedTerm(null); }, 600);
                    }
                }
            }, [selectedExample, selectedTerm]);

            if (quizFinished) return (
                <div className="min-h-screen flex items-center justify-center p-6 text-center bg-slate-950">
                    <div className="bg-slate-900 p-12 rounded-[3rem] border border-white/10 fade-in max-w-lg w-full shadow-2xl">
                        <Icons.Trophy size={80} />
                        <h2 className="text-5xl font-black mt-8 tracking-tighter uppercase italic text-white">Sesión Terminada</h2>
                        <p className="text-2xl text-slate-400 mt-4">Aciertos: <span className="text-white font-black">{score}</span></p>
                        <button onClick={() => setQuizMode(false)} className="mt-10 bg-indigo-600 hover:bg-indigo-500 w-full py-5 rounded-2xl font-bold uppercase transition-all transform hover:scale-105 text-white">Volver a la Guía</button>
                    </div>
                </div>
            );

            if (quizMode) return (
                <div className="min-h-screen p-4 md:p-10 flex flex-col items-center bg-slate-950">
                    <div className="w-full max-w-4xl bg-slate-900 p-8 md:p-12 rounded-[2.5rem] border border-white/10 relative fade-in shadow-2xl">
                        <button onClick={() => setQuizMode(false)} className="absolute top-6 right-6 text-slate-500 hover:text-white transition-colors p-2 hover:bg-white/10 rounded-full"><Icons.X /></button>
                        
                        {quizType === 'flashcards' ? (
                            <div className="flex flex-col items-center mt-6">
                                <span className="bg-purple-500/20 px-4 py-1 rounded-full text-purple-400 text-xs font-bold uppercase mb-8 tracking-widest">Tarjeta {flashcardIndex + 1} / {questions.length}</span>
                                <div className="w-full max-w-md aspect-[4/3] cursor-pointer group perspective-1000" onClick={() => setIsFlipped(!isFlipped)}>
                                    <div className={`relative w-full h-full transition-all duration-500 transform-style-3d ${isFlipped ? 'rotate-y-180' : ''}`}>
                                        <div className="absolute inset-0 bg-slate-800 rounded-[2rem] border border-white/10 flex flex-col items-center justify-center p-8 text-center backface-hidden shadow-xl">
                                            <p className="text-xl italic whitespace-pre-line leading-relaxed text-white">"{questions[flashcardIndex]?.q}"</p>
                                            <p className="mt-4 text-slate-400 text-sm font-medium">— {questions[flashcardIndex]?.autor}</p>
                                            <p className="mt-8 text-slate-500 text-[10px] font-bold uppercase tracking-widest">Toca para revelar</p>
                                        </div>
                                        <div className="absolute inset-0 bg-indigo-600 rounded-[2rem] flex flex-col items-center justify-center p-8 text-center backface-hidden rotate-y-180 shadow-xl">
                                            <h4 className="text-4xl font-black uppercase tracking-tighter text-white">{questions[flashcardIndex]?.correct}</h4>
                                        </div>
                                    </div>
                                </div>
                                <div className="flex gap-4 mt-12 w-full max-w-md">
                                    <button onClick={() => { setFlashcardIndex(Math.max(0, flashcardIndex-1)); setIsFlipped(false); }} className="flex-1 py-4 bg-white/5 rounded-2xl font-bold hover:bg-white/10 transition-colors text-white">Anterior</button>
                                    <button onClick={() => { if(flashcardIndex < questions.length - 1) { setFlashcardIndex(flashcardIndex+1); setIsFlipped(false); } else setQuizFinished(true); }} className="flex-1 py-4 bg-indigo-600 rounded-2xl font-bold hover:bg-indigo-500 transition-colors text-white">Siguiente</button>
                                </div>
                            </div>
                        ) : quizType === 'matching' ? (
                            <div className="mt-10">
                                <div className="flex justify-between mb-8"><h3 className="text-indigo-400 font-bold uppercase text-xs tracking-widest italic">Emparejar</h3><span className="text-slate-500 font-bold">{matchingPairs.length} restantes</span></div>
                                <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                                    <div className="space-y-4">
                                        {matchingPairs.map((p, i) => (
                                            <button key={i} onClick={() => setSelectedExample(p)} className={`w-full p-6 rounded-2xl border text-left transition-all ${selectedExample?.q === p.q ? 'bg-indigo-600 border-indigo-400 scale-[1.02]' : 'bg-slate-800 border-white/5 hover:bg-slate-700'}`}>
                                                <p className="italic text-sm whitespace-pre-line leading-snug text-white">"{p.q}"</p>
                                                <p className="mt-2 text-xs text-slate-500">— {p.autor}</p>
                                            </button>
                                        ))}
                                    </div>
                                    <div className="space-y-4">
                                        {[...matchingPairs].sort((a,b) => a.correct.localeCompare(b.correct)).map((p, i) => (
                                            <button key={i} onClick={() => setSelectedTerm(p)} className={`w-full p-6 h-full min-h-[70px] rounded-2xl border font-black transition-all ${selectedTerm?.correct === p.correct ? 'bg-indigo-600 border-indigo-400 scale-[1.02]' : 'bg-slate-800 border-white/5 hover:bg-slate-700'} text-white`}>
                                                {p.correct}
                                            </button>
                                        ))}
                                    </div>
                                </div>
                            </div>
                        ) : (
                            <div className="text-center mt-10">
                                <span className="bg-emerald-500/20 px-4 py-1 rounded-full text-emerald-400 text-xs font-bold uppercase mb-8 inline-block tracking-widest">Pregunta {currentQuestion+1} / {questions.length}</span>
                                <p className="text-2xl md:text-3xl italic mb-4 whitespace-pre-line leading-relaxed text-white">"{questions[currentQuestion]?.q}"</p>
                                <p className="text-slate-400 text-lg mb-12">— {questions[currentQuestion]?.autor}</p>
                                <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                                    {questions[currentQuestion]?.options.map((o, i) => (
                                        <button key={i} onClick={() => { if(o === questions[currentQuestion].correct) setScore(score+1); if(currentQuestion < questions.length-1) setCurrentQuestion(currentQuestion+1); else setQuizFinished(true); }} className="py-5 bg-slate-800 hover:bg-indigo-600 rounded-2xl border border-white/5 font-bold transition-all transform hover:scale-[1.02] text-lg uppercase text-white">{o}</button>
                                    ))}
                                </div>
                            </div>
                        )}
                    </div>
                </div>
            );

            return (
                <div className="max-w-6xl mx-auto p-6 md:p-12 fade-in flex flex-col min-h-screen">
                    <div className="flex-grow">
                        <header className="text-center mb-16">
                            <div className="inline-flex items-center gap-3 px-4 py-2 bg-indigo-500/10 border border-indigo-500/20 rounded-full mb-6 text-indigo-400 font-bold text-xs uppercase tracking-widest">
                                <Icons.Sparkles /> PAU 2026
                            </div>
                            <h1 className="text-6xl md:text-9xl font-black tracking-tighter mb-10 bg-clip-text text-transparent bg-gradient-to-b from-white to-slate-500 uppercase">RETÓRICA <span className="text-indigo-500 italic">ULTRA</span></h1>
                            
                            <div className="flex flex-wrap justify-center gap-3 mb-12">
                                <button onClick={() => {setActiveTab('sintaxis'); setSearchTerm('');}} className={`px-8 py-4 rounded-2xl font-black uppercase transition-all transform hover:scale-105 ${activeTab === 'sintaxis' && !searchTerm ? 'bg-indigo-600 shadow-lg' : 'bg-slate-900 border border-white/10 text-slate-400'}`}>Sintaxis</button>
                                <button onClick={() => {setActiveTab('semantica'); setSearchTerm('');}} className={`px-8 py-4 rounded-2xl font-black uppercase transition-all transform hover:scale-105 ${activeTab === 'semantica' && !searchTerm ? 'bg-rose-600 shadow-lg' : 'bg-slate-900 border border-white/10 text-slate-400'}`}>Semántica</button>
                                <div className="w-px h-12 bg-white/10 mx-2 hidden md:block self-center"></div>
                                <button onClick={() => startQuiz('choice')} className="px-6 py-4 rounded-2xl font-black uppercase bg-emerald-600 hover:bg-emerald-500 shadow-lg transform hover:scale-105 transition-all text-white">Test</button>
                                <button onClick={() => startQuiz('matching')} className="px-6 py-4 rounded-2xl font-black uppercase bg-amber-600 hover:bg-amber-500 shadow-lg transform hover:scale-105 transition-all text-white">Emparejar</button>
                                <button onClick={() => startQuiz('flashcards')} className="px-6 py-4 rounded-2xl font-black uppercase bg-purple-600 hover:bg-purple-500 shadow-lg transform hover:scale-105 transition-all text-white">Tarjetas</button>
                            </div>

                            <div className="relative group max-w-2xl mx-auto">
                                <div className="absolute left-6 top-1/2 -translate-y-1/2 text-slate-500 group-focus-within:text-indigo-500 transition-colors"><Icons.Search /></div>
                                <input 
                                    type="text" 
                                    placeholder="Busca figura, poeta o verso (ej: Bécquer)..." 
                                    className="w-full p-6 pl-16 bg-slate-900 rounded-full border-2 border-white/5 outline-none focus:border-indigo-500 transition-all text-xl shadow-2xl focus:shadow-indigo-500/10 text-white" 
                                    value={searchTerm} 
                                    onChange={e => setSearchTerm(e.target.value)} 
                                />
                            </div>
                        </header>

                        <div className="space-y-24">
                            {[...CATEGORIAS_DATA.sintaxis, ...CATEGORIAS_DATA.semantica].map((grupo, i) => {
                                const normSearch = normalizeText(searchTerm);
                                const items = grupo.items.filter(item => 
                                    normalizeText(item.nombre).includes(normSearch) || 
                                    normalizeText(item.def).includes(normSearch) ||
                                    normalizeText(item.autor).includes(normSearch) ||
                                    normalizeText(item.autorAlt).includes(normSearch)
                                );
                                
                                const esSintaxis = CATEGORIAS_DATA.sintaxis.some(g => g.grupo === grupo.grupo);
                                if (items.length === 0 || (!searchTerm && ((activeTab === 'sintaxis' && !esSintaxis) || (activeTab === 'semantica' && esSintaxis)))) return null;
                                
                                return (
                                    <div key={i} className="fade-in">
                                        <div className="flex items-center gap-4 mb-8">
                                            <div className={`w-2 h-10 rounded-full ${grupo.color}`}></div>
                                            <h2 className="text-3xl font-black uppercase tracking-tight text-white/90 italic">{grupo.grupo}</h2>
                                        </div>
                                        <div className="grid gap-10">
                                            {items.map((item, j) => {
                                                const IconComponent = Icons[item.iconName] || Icons.Book;
                                                return (
                                                    <div key={j} className="group bg-slate-900 p-10 md:p-12 rounded-[3rem] border border-white/10 hover:border-white/20 transition-all shadow-xl hover:shadow-indigo-500/5">
                                                        <div className="flex flex-col lg:flex-row gap-12">
                                                            <div className="flex-1">
                                                                <div className={`w-14 h-14 rounded-2xl flex items-center justify-center ${grupo.color} mb-8 shadow-lg`}><IconComponent /></div>
                                                                <h3 className="text-5xl font-black mb-4 tracking-tighter uppercase italic text-white">{item.nombre}</h3>
                                                                <p className="text-slate-400 text-xl mb-10 leading-relaxed font-medium">{item.def}</p>
                                                                <div className="space-y-6">
                                                                    <div className="bg-black/40 p-8 rounded-[2rem] border-l-4 border-indigo-500 shadow-inner">
                                                                        <div className="flex justify-between items-start mb-4">
                                                                            <span className="text-[10px] font-bold text-indigo-400 uppercase tracking-widest block italic">Ejemplo Principal</span>
                                                                            <span className="text-[10px] font-black text-slate-500 uppercase italic">— {item.autor}</span>
                                                                        </div>
                                                                        <p className="text-indigo-50 italic text-2xl leading-snug whitespace-pre-line">"{item.ej}"</p>
                                                                    </div>
                                                                    <div className="bg-white/5 p-8 rounded-[2rem] border-l-4 border-emerald-500 border-dashed">
                                                                        <div className="flex justify-between items-start mb-4">
                                                                            <span className="text-[10px] font-bold text-emerald-400 uppercase tracking-widest block italic">Ejemplo Alternativo</span>
                                                                            <span className="text-[10px] font-black text-slate-500 uppercase italic">— {item.autorAlt}</span>
                                                                        </div>
                                                                        <p className="text-slate-300 italic whitespace-pre-line text-xl leading-relaxed">"{item.ejAlt}"</p>
                                                                    </div>
                                                                </div>
                                                            </div>
                                                            <div className="lg:w-1/3 bg-indigo-500/10 border border-indigo-500/20 rounded-[2.5rem] p-10 flex flex-col justify-center">
                                                                <div className="flex items-center gap-3 text-indigo-400 font-black uppercase text-xs tracking-widest mb-6 tracking-widest">
                                                                    <Icons.Graduation /> El Consejo del Maestro
                                                                </div>
                                                                <p className="text-indigo-100 text-lg leading-relaxed italic font-medium">"{item.maestro}"</p>
                                                            </div>
                                                        </div>
                                                    </div>
                                                );
                                            })}
                                        </div>
                                    </div>
                                );
                            })}
                        </div>
                    </div>

                    <footer className="mt-24 border-t border-white/10 pt-16 pb-12 text-center text-slate-500">
                        <p className="mb-4 font-black text-slate-300 tracking-widest uppercase text-sm tracking-widest">© 2026 Georgina Via</p>
                        <div className="flex justify-center items-center gap-3 text-xs font-medium">
                            <Icons.CreativeCommons />
                            <span className="hover:text-white transition-colors cursor-default tracking-widest uppercase">Licencia Creative Commons Atribución 4.0 Internacional</span>
                        </div>
                    </footer>
                </div>
            );
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
