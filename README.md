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

import { useState } from "react";

export default function ClaveX() {
  const [profile, setProfile] = useState({
    name: "Usuario",
    status: "En línea",
    photo: null,
  });
  const [contacts, setContacts] = useState([
    { name: "Alba", status: "En línea", id: 1 },
    { name: "Carlos", status: "No disponible", id: 2 },
    { name: "Ana", status: "En línea", id: 3 },
  ]);

  const [isProfileModalOpen, setIsProfileModalOpen] = useState(false);
  const [isSettingsModalOpen, setIsSettingsModalOpen] = useState(false);

  const toggleProfileModal = () => setIsProfileModalOpen(!isProfileModalOpen);
  const toggleSettingsModal = () => setIsSettingsModalOpen(!isSettingsModalOpen);

  const openChat = (contact) => {
    console.log(`Iniciando chat con ${contact.name}`);
  };

  return (
    <div className="min-h-screen bg-gray-900 text-white p-4">
      <div className="flex justify-between items-center mb-4">
        <button
          onClick={toggleProfileModal}
          className="text-lg font-bold text-purple-600"
        >
          {profile.name}
        </button>
        <button
          onClick={toggleSettingsModal}
          className="text-white text-2xl"
        >
          ⚙️
        </button>
      </div>

      <div className="flex justify-between items-center bg-gray-800 p-2 rounded-lg mb-4">
        <button
          onClick={() => console.log("Agregar contacto")}
          className="bg-purple-600 text-white py-2 px-4 rounded-lg"
        >
          Agregar Contacto
        </button>
      </div>

      <div className="bg-gray-800 p-4 rounded-lg h-72 overflow-auto">
        {contacts.map((contact) => (
          <div
            key={contact.id}
            onClick={() => openChat(contact)}
            className="flex justify-between items-center p-2 bg-gray-700 rounded-lg mb-2 cursor-pointer"
          >
            <div>
              <h2>{contact.name}</h2>
              <p className="text-sm text-gray-400">{contact.status}</p>
            </div>
            <button className="text-blue-500">Abrir chat</button>
          </div>
        ))}
      </div>

      {isProfileModalOpen && (
        <div className="absolute top-0 left-0 w-full h-full bg-black opacity-50">
          <div className="absolute top-10 left-1/4 w-1/2 bg-gray-800 p-4 rounded-lg">
            <h2 className="text-lg text-purple-600 mb-2">Editar Perfil</h2>
            {/* Aquí se colocarían los campos para cambiar la foto, nombre y estado */}
            <button onClick={toggleProfileModal} className="text-red-500">
              Cerrar
            </button>
          </div>
        </div>
      )}

      {isSettingsModalOpen && (
        <div className="absolute top-0 left-0 w-full h-full bg-black opacity-50">
          <div className="absolute top-10 left-1/4 w-1/2 bg-gray-800 p-4 rounded-lg">
            <h2 className="text-lg text-purple-600 mb-2">Configuraciones</h2>
            {/* Aquí irían las opciones de cerrar sesión, cambiar cuenta, etc. */}
            <button onClick={toggleSettingsModal} className="text-red-500">
              Cerrar
            </button>
          </div>
        </div>
      )}
    </div>
  );
}

import { useEffect, useState } from "react";
import { getFirestore, collection, addDoc, query, orderBy, onSnapshot } from "firebase/firestore";
import { initializeApp } from "firebase/app";
import firebaseConfig from "../firebaseConfig";

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

export default function Home() {
  const [messages, setMessages] = useState([]);
  const [newMessage, setNewMessage] = useState("");

  useEffect(() => {
    const q = query(collection(db, "messages"), orderBy("timestamp", "asc"));
    const unsubscribe = onSnapshot(q, (snapshot) => {
      setMessages(snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() })));
    });

    return () => unsubscribe();
  }, []);

  const sendMessage = async () => {
    if (newMessage.trim() === "") return;
    await addDoc(collection(db, "messages"), { text: newMessage, timestamp: new Date() });
    setNewMessage("");
  };

  return (
    <div className="min-h-screen bg-gray-900 text-white flex flex-col items-center p-4">
      <h1 className="text-xl font-bold">Chat Privado</h1>
      <div className="w-full max-w-lg bg-gray-800 p-4 rounded-lg flex flex-col h-[500px] overflow-auto">
        {messages.map((msg) => (
          <p key={msg.id} className="p-2 bg-gray-700 rounded-lg my-1">
            {msg.user}: {msg.text}
          </p>
        ))}
      </div>
      <div className="w-full max-w-lg flex mt-2">
        <input
          className="flex-1 p-2 bg-gray-700 text-white rounded-l-lg"
          value={newMessage}
          onChange={(e) => setNewMessage(e.target.value)}
          placeholder="Escribe un mensaje..."
        />
        <button onClick={sendMessage} className="bg-blue-500 px-4 rounded-r-lg">
          Enviar
        </button>
      </div>
    </div>
  );
}npm install axios express body-parser

// firebaseConfig.js import { initializeApp } from "firebase/app"; import { getFirestore } from "firebase/firestore";

const firebaseConfig = { apiKey: "TU_API_KEY", authDomain: "TU_AUTH_DOMAIN", projectId: "TU_PROJECT_ID", storageBucket: "TU_STORAGE_BUCKET", messagingSenderId: "TU_MESSAGING_SENDER_ID", appId: "TU_APP_ID", };

const app = initializeApp(firebaseConfig); const db = getFirestore(app);

export { db };

// pages/index.js import { useEffect, useState } from "react"; import { db } from "../firebaseConfig"; import { collection, addDoc, query, orderBy, onSnapshot, serverTimestamp } from "firebase/firestore";

export default function Home() { const [messages, setMessages] = useState([]); const [newMessage, setNewMessage] = useState("");

useEffect(() => { const q = query(collection(db, "messages"), orderBy("timestamp", "asc")); const unsubscribe = onSnapshot(q, (snapshot) => { setMessages(snapshot.docs.map((doc) => ({ id: doc.id, ...doc.data() }))); }); return () => unsubscribe(); }, []);

const sendMessage = async () => { if (!newMessage.trim()) return; await addDoc(collection(db, "messages"), { text: newMessage, timestamp: serverTimestamp(), }); setNewMessage(""); };

return ( <div className="min-h-screen bg-gray-900 text-white flex flex-col items-center p-4"> <h1 className="text-xl font-bold">Chat Privado</h1> <div className="w-full max-w-lg bg-gray-800 p-4 rounded-lg flex flex-col h-96 overflow-auto"> {messages.map((msg) => ( <p key={msg.id} className="p-2 bg-gray-700 rounded-lg my-1">{msg.text}</p> ))} </div> <div className="w-full max-w-lg flex mt-2"> <input className="flex-1 p-2 bg-gray-700 text-white rounded-l-lg" value={newMessage} onChange={(e) => setNewMessage(e.target.value)} placeholder="Escribe un mensaje..." /> <button onClick={sendMessage} className="bg-blue-500 px-4 rounded-r-lg">Enviar</button> </div> </div> ); }

// server.js const express = require("express"); const bodyParser = require("body-parser"); const admin = require("firebase-admin");

const serviceAccount = require("./serviceAccountKey.json"); admin.initializeApp({ credential: admin.credential.cert(serviceAccount) }); const db = admin.firestore();

const app = express(); const port = 5000;

app.use(bodyParser.json());

app.post("/whatsapp-webhook", async (req, res) => { const { messages } = req.body; if (messages && messages.length > 0) { const message = messages[0]; await db.collection("messages").add({ text: message.text.body, user: message.from, timestamp: admin.firestore.FieldValue.serverTimestamp(), }); res.sendStatus(200); } else { res.sendStatus(400); } });

app.listen(port, () => console.log(Servidor corriendo en http://localhost:${port}));

import socket
import threading
import json
import bcrypt
import sqlite3

# Configurar la base de datos
conn = sqlite3.connect("usuarios.db", check_same_thread=False)
cursor = conn.cursor()
cursor.execute("""
    CREATE TABLE IF NOT EXISTS usuarios (
        nombre TEXT PRIMARY KEY,
        password TEXT
    )
""")
conn.commit()

mensajes_eliminados = {}

def registrar_usuario(nombre, password):
    cursor.execute("SELECT * FROM usuarios WHERE nombre=?", (nombre,))
    if cursor.fetchone():
        return False, "El usuario ya existe."

    hashed_password = bcrypt.hashpw(password.encode(), bcrypt.gensalt()).decode()
    cursor.execute("INSERT INTO usuarios (nombre, password) VALUES (?, ?)", (nombre, hashed_password))
    conn.commit()
    return True, "Usuario registrado con éxito."

def autenticar_usuario(nombre, password):
    cursor.execute("SELECT password FROM usuarios WHERE nombre=?", (nombre,))
    user = cursor.fetchone()
    if user and bcrypt.checkpw(password.encode(), user[0].encode()):
        return True, "Autenticación exitosa."
    return False, "Usuario o contraseña incorrectos."

def agregar_contacto(usuario, contacto):
    cursor.execute("SELECT * FROM usuarios WHERE nombre=?", (contacto,))
    if cursor.fetchone():
        return True, f"{contacto} añadido a tu lista de contactos."
    return False, "El contacto no existe."

def ver_mensajes_eliminados(usuario):
    return mensajes_eliminados.get(usuario, [])

def manejar_cliente(cliente_socket, direccion):
    try:
        while True:
            mensaje = cliente_socket.recv(1024).decode("utf-8")
            if not mensaje:
                break

            datos = json.loads(mensaje)
            tipo = datos.get("tipo")
            usuario = datos.get("usuario")

            if tipo == "registrar":
                exito, respuesta = registrar_usuario(usuario, datos.get("password"))
            elif tipo == "autenticar":
                exito, respuesta = autenticar_usuario(usuario, datos.get("password"))
            elif tipo == "agregar_contacto":
                exito, respuesta = agregar_contacto(usuario, datos.get("contacto"))
            elif tipo == "ver_mensajes_eliminados":
                respuesta = ver_mensajes_eliminados(usuario)
                exito = True
            else:
                exito, respuesta = False, "Acción desconocida."

            cliente_socket.send(json.dumps({"exito": exito, "respuesta": respuesta}).encode("utf-8"))
    except Exception as e:
        print(f"Error con el cliente {direccion}: {e}")
    finally:
        cliente_socket.close()

def iniciar_servidor(puerto=12345):
    try:
        servidor = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        servidor.bind(("0.0.0.0", puerto))
        servidor.listen(5)
        print(f"Servidor escuchando en el puerto {puerto}")

        while True:
            cliente_socket, direccion = servidor.accept()
            print(f"Conexión entrante de {direccion}")
            hilo_cliente = threading.Thread(target=manejar_cliente, args=(cliente_socket, direccion))
            hilo_cliente.start()
    except Exception as e:
        print(f"Error en el servidor: {e}")

if __name__ == "__main__":
    iniciar_servidor()
