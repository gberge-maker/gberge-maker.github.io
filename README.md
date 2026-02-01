# gberge-maker.github.io
Recursos educativos de Lengua Castellana y Literatura
import React, { useState, useEffect } from 'react';
import { 
  Sparkles, 
  Repeat, 
  MinusCircle, 
  Shuffle, 
  MessageSquare, 
  Eye, 
  Zap, 
  Search,
  Layout,
  Type,
  Maximize2,
  PenTool,
  CheckCircle2,
  XCircle,
  ExternalLink,
  RotateCcw,
  Trophy,
  Gamepad2,
  ListChecks,
  Keyboard
} from 'lucide-react';

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
  const [userInput, setUserInput] = useState('');
  const [matchingPairs, setMatchingPairs] = useState([]);
  const [selectedExample, setSelectedExample] = useState(null);
  const [selectedTerm, setSelectedTerm] = useState(null);

  // LISTA ACTUALIZADA CON LAS FIGURAS SOLICITADAS
  const figurasOficiales = [
    "Anáfora", "Aliteración", "Paralelismo", "Hipérbaton", "Quiasmo", 
    "Paronomasia", "Asíndeton", "Polisíndeton", "Metáfora", "Metonimia", 
    "Símil", "Sinestesia", "Hipérbole", "Personificación", "Antítesis", 
    "Paradoja", "Oxímoron", "Epíteto", "Políptoton", "Epanadiplosis", 
    "Epífora", "Zeugma"
  ];

  const categorias = {
    sintaxis: [
      {
        grupo: "Repetición y Ritmo (Nivel Fónico)",
        color: "bg-blue-500",
        items: [
          { nombre: "Anáfora", def: "Repetición de palabras al comienzo de varios versos.", ej: "Temprano levantó la muerte el vuelo, / temprano madrugó la madrugada.", icon: <Repeat className="w-5 h-5" /> },
          { nombre: "Aliteración", def: "Repetición de sonidos similares para evocar una sensación.", ej: "En el silencio solo se escuchaba / un susurro de abejas que sonaba.", icon: <Zap className="w-5 h-5" /> },
          { nombre: "Paralelismo", def: "Repetición de una misma estructura sintáctica.", ej: "La tierra más que el cuerpo se resiente; / la sangre más que el alma se dilata.", icon: <Layout className="w-5 h-5" /> },
          { nombre: "Políptoton", def: "Repetición de una palabra con diferentes morfemas flexivos (género, número, tiempo).", ej: "¡Vive Dios, que la he de ver, / veréis si la veré yo!", icon: <Type className="w-5 h-5" /> },
          { nombre: "Epanadiplosis", def: "Repetición de la misma palabra al principio y al final de un verso.", ej: "Verde que te quiero verde.", icon: <Repeat className="w-5 h-5" /> },
          { nombre: "Epífora", def: "Repetición de palabras al final de versos o frases consecutivas.", ej: "No decía palabras, / acercaba tan solo un cuerpo como un cuerpo.", icon: <Repeat className="w-5 h-5" /> }
        ]
      },
      {
        grupo: "Orden y Estructura",
        color: "bg-indigo-600",
        items: [
          { nombre: "Hipérbaton", def: "Alteración del orden lógico gramatical de las palabras.", ej: "Volverán las oscuras golondrinas / en tu balcón sus nidos a colgar.", icon: <Shuffle className="w-5 h-5" /> },
          { nombre: "Quiasmo", def: "Repetición de estructuras cruzando su orden (A-B / B-A).", ej: "Ni son todos los que están, / ni están todos los que son.", icon: <Shuffle className="w-5 h-5" /> },
          { nombre: "Paronomasia", def: "Uso de palabras de sonido similar pero distinto significado.", ej: "Vendado que me has vendido. / Entre casar y cansar.", icon: <Type className="w-5 h-5" /> }
        ]
      },
      {
        grupo: "Adición u Omisión",
        color: "bg-cyan-500",
        items: [
          { nombre: "Asíndeton", def: "Omisión de conjunciones para dar rapidez y dinamismo.", ej: "Llegué, vi, vencí.", icon: <MinusCircle className="w-5 h-5" /> },
          { nombre: "Polisíndeton", def: "Repetición innecesaria de conjunciones para dar lentitud o solemnidad.", ej: "Y ríe, y llora, y aborrece, y ama.", icon: <Zap className="w-5 h-5" /> },
          { nombre: "Zeugma", def: "Omisión de un elemento (normalmente un verbo) que ya ha aparecido.", ej: "La niña es rubia; el niño, moreno.", icon: <MinusCircle className="w-5 h-5" /> }
        ]
      }
    ],
    semantica: [
      {
        grupo: "Relaciones de Significado (Tropos)",
        color: "bg-rose-500",
        items: [
          { nombre: "Metáfora", def: "Identificación de un término real con uno imaginario.", ej: "Las perlas de tu boca.", icon: <Sparkles className="w-5 h-5" /> },
          { nombre: "Metonimia", def: "Sustitución basada en relaciones de proximidad (autor por obra, continente por contenido).", ej: "Se lee a Cervantes.", icon: <Shuffle className="w-5 h-5" /> },
          { nombre: "Símil", def: "Comparación explícita usando 'como' o nexos similares.", ej: "Tus ojos son como luceros.", icon: <Eye className="w-5 h-5" /> },
          { nombre: "Sinestesia", def: "Atribución de una sensación a un sentido que no le corresponde.", ej: "Escucho con los ojos a los muertos.", icon: <Zap className="w-5 h-5" /> }
        ]
      },
      {
        grupo: "Figuras de Pensamiento",
        color: "bg-amber-500",
        items: [
          { nombre: "Hipérbole", def: "Exageración de la realidad para aumentar su impacto.", ej: "Érase un hombre a una nariz pegado.", icon: <Maximize2 className="w-5 h-5" /> },
          { nombre: "Personificación", def: "Atribuir rasgos humanos a objetos o conceptos abstractos.", ej: "La lluvia lloraba sobre el cristal.", icon: <Eye className="w-5 h-5" /> },
          { nombre: "Antítesis", def: "Oposición de dos ideas empleando palabras de significado contrario.", ej: "Es tan corto el amor y tan largo el olvido.", icon: <Shuffle className="w-5 h-5" /> },
          { nombre: "Paradoja", def: "Unión de ideas contradictorias que encierran un sentido profundo.", ej: "Vivo sin vivir en mí.", icon: <Zap className="w-5 h-5" /> },
          { nombre: "Oxímoron", def: "Contradicción entre dos términos contiguos.", ej: "Hielo abrasador.", icon: <MinusCircle className="w-5 h-5" /> },
          { nombre: "Epíteto", def: "Adjetivo que expresa una cualidad intrínseca del sustantivo.", ej: "Blanca nieve.", icon: <Sparkles className="w-5 h-5" /> }
        ]
      }
    ]
  };

  // Banco de preguntas actualizado con las nuevas figuras
  const masterBank = [
    { q: "Temprano levantó la muerte el vuelo, / temprano madrugó la madrugada", correct: "Anáfora" },
    { q: "Vendado que me has vendido", correct: "Paronomasia" },
    { q: "En el silencio solo se escuchaba / un susurro de abejas que sonaba", correct: "Aliteración" },
    { q: "La tierra más que el cuerpo se resiente; / la sangre más que el alma se dilata", correct: "Paralelismo" },
    { q: "Ni son todos los que están, / ni están todos los que son", correct: "Quiasmo" },
    { q: "Volverán las oscuras golondrinas / en tu balcón sus nidos a colgar", correct: "Hipérbaton" },
    { q: "Es tan corto el amor y tan largo el olvido", correct: "Antítesis" },
    { q: "Escucho con los ojos a los muertos", correct: "Sinestesia" },
    { q: "Érase un hombre a una nariz pegado", correct: "Hipérbole" },
    { q: "Vivo sin vivir en mí", correct: "Paradoja" },
    { q: "Hielo abrasador, fuego helado", correct: "Oxímoron" },
    { q: "Llegué, vi, vencí", correct: "Asíndeton" },
    { q: "Y ríe, y llora, y aborrece, y ama", correct: "Polisíndeton" },
    { q: "Blanca nieve, verde hierba", correct: "Epíteto" },
    { q: "Cádiz, salada claridad", correct: "Sinestesia" },
    { q: "Se lee a Cervantes", correct: "Metonimia" },
    { q: "¡Oh muerte que das vida!", correct: "Paradoja" },
    { q: "Tus ojos son como luceros", correct: "Símil" },
    { q: "A las aladas almas de las rosas", correct: "Aliteración" },
    { q: "Pena con pena y pena desayuno / pena es mi paz y pena mi batalla", correct: "Anáfora" },
    { q: "La lluvia lloraba sobre el cristal", correct: "Personificación" },
    { q: "Verde que te quiero verde", correct: "Epanadiplosis" },
    { q: "¡Vive Dios, que la he de ver, / veréis si la veré yo!", correct: "Políptoton" },
    { q: "La niña es rubia; el niño, moreno", correct: "Zeugma" },
    { q: "No decía palabras, / acercaba tan solo un cuerpo como un cuerpo.", correct: "Epífora" },
    { q: "Pasos de un peregrino son, errante", correct: "Hipérbaton" },
    { q: "El viento se llevó los días", correct: "Metáfora" },
    { q: "Tanto dolor se agrupa en mi costado", correct: "Hipérbole" },
    { q: "Corren las nubes, vuela el tiempo", correct: "Paralelismo" },
    { q: "La soledad sonora", correct: "Oxímoron" }
  ];

  const getOptions = (correct) => {
    let filtered = figurasOficiales.filter(f => f !== correct);
    let shuffled = filtered.sort(() => 0.5 - Math.random()).slice(0, 2);
    return [...shuffled, correct].sort();
  };

  const startQuiz = (type) => {
    const shuffled = [...masterBank].sort(() => 0.5 - Math.random()).slice(0, 10);
    if (type === 'matching') {
      setMatchingPairs(shuffled.slice(0, 5));
    } else {
      setQuestions(shuffled.map(item => ({ ...item, options: getOptions(item.correct) })));
    }
    setQuizType(type);
    setQuizMode(true);
    setScore(0);
    setCurrentQuestion(0);
    setQuizFinished(false);
    setShowResult(null);
    setUserInput('');
  };

  const handleChoice = (opt) => {
    if (opt === questions[currentQuestion].correct) {
      setScore(score + 1);
      setShowResult('correct');
    } else {
      setShowResult('incorrect');
    }
    setTimeout(() => {
      setShowResult(null);
      if (currentQuestion < questions.length - 1) setCurrentQuestion(currentQuestion + 1);
      else setQuizFinished(true);
    }, 800);
  };

  const handleTyping = (e) => {
    e.preventDefault();
    if (userInput.toLowerCase().trim() === questions[currentQuestion].correct.toLowerCase().trim()) {
      setScore(score + 1);
      setShowResult('correct');
    } else {
      setShowResult('incorrect');
    }
    setTimeout(() => {
      setShowResult(null);
      setUserInput('');
      if (currentQuestion < questions.length - 1) setCurrentQuestion(currentQuestion + 1);
      else setQuizFinished(true);
    }, 1000);
  };

  useEffect(() => {
    if (selectedExample && selectedTerm) {
      if (selectedExample.correct === selectedTerm.correct) {
        setScore(score + 1);
        const newPairs = matchingPairs.filter(p => p.q !== selectedExample.q);
        setMatchingPairs(newPairs);
        if (newPairs.length === 0) setQuizFinished(true);
      } else {
        setShowResult('incorrect');
        setTimeout(() => setShowResult(null), 500);
      }
      setSelectedExample(null);
      setSelectedTerm(null);
    }
  }, [selectedExample, selectedTerm]);

  return (
    <div className="min-h-screen bg-slate-950 text-slate-100 font-sans selection:bg-indigo-500 selection:text-white">
      <div className="fixed inset-0 overflow-hidden pointer-events-none opacity-20">
        <div className="absolute top-[-10%] left-[-10%] w-[40%] h-[40%] bg-indigo-600 rounded-full blur-[120px]"></div>
        <div className="absolute bottom-[-10%] right-[-10%] w-[40%] h-[40%] bg-rose-600 rounded-full blur-[120px]"></div>
      </div>

      <header className="relative max-w-6xl mx-auto pt-12 pb-8 px-6 text-center z-10">
        <div className="inline-flex items-center gap-3 px-4 py-2 bg-indigo-500/10 border border-indigo-500/20 rounded-full mb-6 text-indigo-400 font-bold text-xs uppercase tracking-widest">
          <Sparkles size={16} /> Contenido Oficial PAU 2024-2025
        </div>
        <h1 className="text-6xl md:text-7xl font-black tracking-tight mb-4 bg-clip-text text-transparent bg-gradient-to-b from-white to-slate-400">
          RETÓRICA <span className="text-indigo-500 italic">PRO</span>
        </h1>

        <div className="mt-12 flex flex-wrap justify-center gap-3">
          <button 
            onClick={() => { setActiveTab('sintaxis'); setQuizMode(false); }}
            className={`px-8 py-4 rounded-2xl font-black uppercase tracking-wider transition-all ${activeTab === 'sintaxis' && !quizMode ? 'bg-indigo-600 shadow-[0_0_30px_rgba(79,70,229,0.4)]' : 'bg-white/5 text-slate-400 hover:bg-white/10'}`}
          >
            Guía Sintaxis
          </button>
          <button 
            onClick={() => { setActiveTab('semantica'); setQuizMode(false); }}
            className={`px-8 py-4 rounded-2xl font-black uppercase tracking-wider transition-all ${activeTab === 'semantica' && !quizMode ? 'bg-rose-600 shadow-[0_0_30px_rgba(225,29,72,0.4)]' : 'bg-white/5 text-slate-400 hover:bg-white/10'}`}
          >
            Guía Semántica
          </button>
          <div className="w-px h-12 bg-white/10 mx-2 hidden md:block"></div>
          <button onClick={() => startQuiz('choice')} className={`px-8 py-4 rounded-2xl font-black uppercase tracking-wider flex items-center gap-2 transition-all ${quizMode && quizType === 'choice' ? 'bg-emerald-600' : 'bg-white/5 text-slate-400 hover:bg-white/10'}`}><ListChecks size={20} /> Test</button>
          <button onClick={() => startQuiz('matching')} className={`px-8 py-4 rounded-2xl font-black uppercase tracking-wider flex items-center gap-2 transition-all ${quizMode && quizType === 'matching' ? 'bg-amber-600' : 'bg-white/5 text-slate-400 hover:bg-white/10'}`}><Gamepad2 size={20} /> Emparejar</button>
          <button onClick={() => startQuiz('typing')} className={`px-8 py-4 rounded-2xl font-black uppercase tracking-wider flex items-center gap-2 transition-all ${quizMode && quizType === 'typing' ? 'bg-indigo-600' : 'bg-white/5 text-slate-400 hover:bg-white/10'}`}><Keyboard size={20} /> Desafío</button>
        </div>
      </header>

      <main className="relative max-w-6xl mx-auto px-6 pb-24 z-10">
        {!quizMode ? (
          <div className="animate-in fade-in duration-700">
            <div className="relative mb-12">
              <Search className="absolute left-6 top-1/2 -translate-y-1/2 text-slate-500" size={24} />
              <input 
                type="text" 
                placeholder={`Busca en ${activeTab === 'sintaxis' ? 'sintaxis' : 'semántica'}...`}
                className="w-full bg-white/5 border-2 border-white/5 focus:border-indigo-500/50 rounded-[2rem] py-6 pl-16 pr-8 outline-none text-xl transition-all backdrop-blur-md"
                value={searchTerm}
                onChange={(e) => setSearchTerm(e.target.value)}
              />
            </div>

            <div className="space-y-16">
              {categorias[activeTab].map((grupo, gIdx) => (
                <section key={gIdx}>
                  <div className="flex items-center gap-6 mb-8">
                    <div className={`h-12 w-2 rounded-full ${grupo.color}`}></div>
                    <h2 className="text-4xl font-black tracking-tight uppercase text-white/90">{grupo.grupo}</h2>
                  </div>
                  <div className="grid grid-cols-1 md:grid-cols-2 gap-8">
                    {grupo.items.filter(i => i.nombre.toLowerCase().includes(searchTerm.toLowerCase())).map((item, iIdx) => (
                      <div key={iIdx} className="group bg-white/5 border border-white/10 rounded-[2.5rem] p-8 hover:bg-white/10 transition-all duration-500">
                        <div className={`mb-6 w-14 h-14 rounded-2xl flex items-center justify-center ${grupo.color} text-white shadow-xl`}>{item.icon}</div>
                        <h3 className="text-2xl font-black mb-3 group-hover:text-indigo-400 transition-colors">{item.nombre}</h3>
                        <p className="text-slate-400 text-sm mb-6 leading-relaxed font-medium">{item.def}</p>
                        <div className="bg-black/40 p-6 rounded-3xl border border-white/5">
                          <span className="text-[10px] font-bold text-indigo-500 uppercase tracking-[0.2em] block mb-2">Ejemplo Seleccionado</span>
                          <p className="text-indigo-50 italic text-lg leading-snug">"{item.ej}"</p>
                        </div>
                      </div>
                    ))}
                  </div>
                </section>
              ))}
            </div>
          </div>
        ) : (
          <div className="max-w-4xl mx-auto py-8">
            {!quizFinished ? (
              <div className="bg-white/5 border border-white/10 rounded-[3.5rem] p-12 relative overflow-hidden backdrop-blur-xl">
                {showResult && (
                  <div className={`absolute inset-0 z-20 flex items-center justify-center backdrop-blur-sm animate-in zoom-in ${showResult === 'correct' ? 'bg-emerald-500/10' : 'bg-rose-500/10'}`}>
                    {showResult === 'correct' ? <CheckCircle2 size={120} className="text-emerald-500" /> : <XCircle size={120} className="text-rose-500" />}
                  </div>
                )}

                {quizType !== 'matching' ? (
                  <>
                    <div className="flex justify-between mb-10 items-center">
                      <span className="bg-indigo-500/20 px-6 py-2 rounded-full font-bold text-indigo-400 text-sm tracking-widest uppercase">PREGUNTA {currentQuestion + 1} / 10</span>
                      <div className="flex items-center gap-2 text-slate-400 font-bold tracking-widest">PUNTOS: <span className="text-white text-xl">{score}</span></div>
                    </div>
                    <div className="min-h-[180px] flex items-center justify-center text-center mb-12">
                      <p className="text-4xl font-serif italic leading-tight text-white">"{questions[currentQuestion]?.q}"</p>
                    </div>

                    {quizType === 'choice' ? (
                      <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
                        {questions[currentQuestion]?.options.map((opt, i) => (
                          <button key={i} onClick={() => handleChoice(opt)} className="py-6 rounded-3xl bg-white/5 border border-white/10 hover:bg-indigo-600 transition-all font-black text-xl">{opt}</button>
                        ))}
                      </div>
                    ) : (
                      <form onSubmit={handleTyping} className="space-y-6">
                        <input autoFocus type="text" placeholder="Escribe el nombre de la figura..." className="w-full bg-black/40 border-2 border-white/10 rounded-3xl py-8 px-10 text-3xl outline-none focus:border-indigo-500 text-center font-bold" value={userInput} onChange={(e) => setUserInput(e.target.value)} />
                        <button type="submit" className="w-full py-6 bg-indigo-600 rounded-3xl font-black uppercase tracking-widest hover:scale-[1.02] transition-transform">Validar Respuesta</button>
                      </form>
                    )}
                  </>
                ) : (
                  <div className="grid grid-cols-1 md:grid-cols-2 gap-12">
                    <div className="space-y-4">
                      <h4 className="text-xs font-black uppercase tracking-widest text-slate-500 mb-6">Ejemplos Literarios</h4>
                      {matchingPairs.map((pair, idx) => (
                        <button key={idx} onClick={() => setSelectedExample(pair)} className={`w-full p-6 rounded-3xl border text-left transition-all ${selectedExample?.q === pair.q ? 'bg-amber-600 border-amber-300 shadow-[0_0_20px_rgba(245,158,11,0.3)]' : 'bg-white/5 border-white/10 hover:bg-white/10'}`}>
                          <p className="italic font-serif text-lg leading-snug">"{pair.q}"</p>
                        </button>
                      ))}
                    </div>
                    <div className="space-y-4">
                      <h4 className="text-xs font-black uppercase tracking-widest text-slate-500 mb-6">Figuras Retóricas</h4>
                      {[...matchingPairs].sort((a,b) => a.correct.localeCompare(b.correct)).map((pair, idx) => (
                        <button key={idx} onClick={() => setSelectedTerm(pair)} className={`w-full p-6 h-full rounded-3xl border text-center font-black text-xl transition-all ${selectedTerm?.correct === pair.correct ? 'bg-amber-600 border-amber-300 shadow-[0_0_20px_rgba(245,158,11,0.3)]' : 'bg-white/5 border-white/10 hover:bg-white/10'}`}>
                          {pair.correct}
                        </button>
                      ))}
                    </div>
                  </div>
                )}
              </div>
            ) : (
              <div className="text-center bg-white/5 border border-white/10 rounded-[4rem] p-20 backdrop-blur-xl animate-in zoom-in-95">
                <Trophy size={120} className="mx-auto text-amber-500 mb-10 drop-shadow-[0_0_30px_rgba(245,158,11,0.5)]" />
                <h2 className="text-6xl font-black mb-6">¡Sesión Terminada!</h2>
                <p className="text-3xl text-slate-400 mb-12">Aciertos totales: <span className="text-white font-bold">{score} de 10</span></p>
                <div className="flex flex-wrap justify-center gap-6">
                  <button onClick={() => startQuiz(quizType)} className="bg-indigo-600 px-12 py-6 rounded-[2rem] font-black uppercase tracking-widest flex items-center gap-4 hover:scale-105 transition-transform"><RotateCcw size={28} /> Reintentar</button>
                  <button onClick={() => setQuizMode(false)} className="bg-white/10 px-12 py-6 rounded-[2rem] font-black uppercase tracking-widest hover:bg-white/20 transition-all">Ver Guía</button>
                </div>
              </div>
            )}
          </div>
        )}
      </main>

      <footer className="text-center pb-20 text-slate-600 text-xs font-black tracking-[0.5em] uppercase opacity-50">
        Laboratorio de Retórica • Terminología Completa PAU
      </footer>
    </div>
  );
};

export default App;

