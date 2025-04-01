# Chat-privado
Aplicación de chat privado basada en Next.js y Firebase con máxima privacidad."
// Archivo: firebaseConfig.js // Configuración de Firebase (reemplaza con tus credenciales) const firebaseConfig = { apiKey: "TU_API_KEY", authDomain: "TU_AUTH_DOMAIN", projectId: "TU_PROJECT_ID", storageBucket: "TU_STORAGE_BUCKET", messagingSenderId: "TU_MESSAGING_SENDER_ID", appId: "TU_APP_ID" };

export default firebaseConfig;

// Archivo: pages/index.js import { useEffect, useState } from "react"; import { getFirestore, collection, addDoc, query, orderBy, onSnapshot } from "firebase/firestore"; import { initializeApp } from "firebase/app"; import firebaseConfig from "../firebaseConfig";

const app = initializeApp(firebaseConfig); const db = getFirestore(app);

export default function Home() { const [messages, setMessages] = useState([]); const [newMessage, setNewMessage] = useState("");

useEffect(() => { const q = query(collection(db, "messages"), orderBy("timestamp", "asc")); const unsubscribe = onSnapshot(q, (snapshot) => { setMessages(snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }))); }); return () => unsubscribe(); }, []);

const sendMessage = async () => { if (newMessage.trim() === "") return; await addDoc(collection(db, "messages"), { text: newMessage, timestamp: new Date() }); setNewMessage(""); };

return ( <div className="min-h-screen bg-gray-900 text-white flex flex-col items-center p-4"> <h1 className="text-xl font-bold">Chat Privado</h1> <div className="w-full max-w-lg bg-gray-800 p-4 rounded-lg flex flex-col h-[500px] overflow-auto"> {messages.map((msg) => ( <p key={msg.id} className="p-2 bg-gray-700 rounded-lg my-1">{msg.text}</p> ))} </div> <div className="w-full max-w-lg flex mt-2"> <input className="flex-1 p-2 bg-gray-700 text-white rounded-l-lg" value={newMessage} onChange={(e) => setNewMessage(e.target.value)} placeholder="Escribe un mensaje..." /> <button
onClick={sendMessage}
className="bg-blue-500 px-4 rounded-r-lg"
>Enviar</button> </div> </div> ); }

