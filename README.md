# Welcome to your Expo app 👋

This is an [Expo](https://expo.dev) project created with [`create-expo-app`](https://www.npmjs.com/package/create-expo-app).

## Get started

1. Install dependencies

   ```bash
   npm install
   ```

2. Start the app

   ```bash
    npx expo start
   ```

In the output, you'll find options to open the app in a

- [development build](https://docs.expo.dev/develop/development-builds/introduction/)
- [Android emulator](https://docs.expo.dev/workflow/android-studio-emulator/)
- [iOS simulator](https://docs.expo.dev/workflow/ios-simulator/)
- [Expo Go](https://expo.dev/go), a limited sandbox for trying out app development with Expo

You can start developing by editing the files inside the **app** directory. This project uses [file-based routing](https://docs.expo.dev/router/introduction).

## Get a fresh project

When you're ready, run:

```bash
npm run reset-project
```

This command will move the starter code to the **app-example** directory and create a blank **app** directory where you can start developing.

## Learn more

To learn more about developing your project with Expo, look at the following resources:

- [Expo documentation](https://docs.expo.dev/): Learn fundamentals, or go into advanced topics with our [guides](https://docs.expo.dev/guides).
- [Learn Expo tutorial](https://docs.expo.dev/tutorial/introduction/): Follow a step-by-step tutorial where you'll create a project that runs on Android, iOS, and the web.

## Join the community

Join our community of developers creating universal apps.

- [Expo on GitHub](https://github.com/expo/expo): View our open source platform and contribute.
- [Discord community](https://chat.expo.dev): Chat with Expo users and ask questions.






# Meu projeto App-Mobile para consumir uma Api-DotNet
# Projeto React Native com Expo que consome uma API .NET com as entidades Aluno, Escola e Professor.
 ## Passo a passo completo, incluindo a criação do projeto e a estrutura básica para consumir os dados da API.


## Template padrão (blank) com JavaScript para criar um app React Native com Expo que consome a API .NET das entidades Aluno, Escola e Professor.

🚀 1. Criar o projeto com template padrão
npx create-expo-app app-escola
cd app-escola


📦 2. Instalar bibliotecas essenciais
npm install axios
npx expo install react-native-screens 
npx expo install react-native-safe-area-context 
npx expo install react-native-gesture-handler 
npx expo install react-native-reanimated



📁 3. Estrutura de arquivos
Crie os seguintes diretórios e arquivos dentro da pasta app-escola:
/app-escola
├── /src
│   ├── /api
│   │   └── api.js
│   ├── /services
│   │   ├── alunoService.js
│   │   ├── escolaService.js
│   │   └── professorService.js
│   ├── /components
│   │   └── AlunoCard.js
│   └── /screens
│       └── HomeScreen.js
├── App.js


🔗 4. api.js
import axios from 'axios';

export const api = axios.create({
  baseURL: 'http://192.168.0.100:5001/api', // altere para o IP da sua API
});


📡 5. Services
alunoService.js
import { api } from '../api/api';

export const getAlunos = async () => {
  const response = await api.get('/alunos');
  return response.data;
};



escolaService.js
import { api } from '../api/api';

export const getEscolas = async () => {
  const response = await api.get('/escolas');
  return response.data;
};



professorService.js
import { api } from '../api/api';

export const getProfessores = async () => {
  const response = await api.get('/professores');
  return response.data;
};



🧩 6. Componente AlunoCard.js
import React from 'react';
import { View, Text, StyleSheet } from 'react-native';

export default function AlunoCard({ aluno }) {
  return (
    <View style={styles.card}>
      <Text style={styles.title}>{aluno.nome}</Text>
      <Text>RMA: {aluno.rma}</Text>
      <Text>Pai: {aluno.pai}</Text>
      <Text>Mãe: {aluno.mae}</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  card: {
    backgroundColor: '#e6e6e6',
    padding: 10,
    marginVertical: 6,
    borderRadius: 6,
  },
  title: {
    fontSize: 16,
    fontWeight: 'bold',
  },
});



📱 7. Tela principal HomeScreen.js
import React, { useEffect, useState } from 'react';
import { View, ScrollView, Text, StyleSheet } from 'react-native';
import { getAlunos } from '../services/alunoService';
import AlunoCard from '../components/AlunoCard';

export default function HomeScreen() {
  const [alunos, setAlunos] = useState([]);

  useEffect(() => {
    const fetchData = async () => {
      const dados = await getAlunos();
      setAlunos(dados);
    };
    fetchData();
  }, []);

  return (
    <ScrollView style={styles.container}>
      <Text style={styles.header}>Lista de Alunos</Text>
      {alunos.map(aluno => (
        <AlunoCard key={aluno.id} aluno={aluno} />
      ))}
    </ScrollView>
  );
}

const styles = StyleSheet.create({
  container: {
    padding: 16,
  },
  header: {
    fontSize: 22,
    fontWeight: 'bold',
    marginBottom: 10,
  },
});


🧠 8. Arquivo principal App.js
import React from 'react';
import { SafeAreaView } from 'react-native';
import HomeScreen from './src/screens/HomeScreen';

export default function App() {
  return (
    <SafeAreaView style={{ flex: 1 }}>
      <HomeScreen />
    </SafeAreaView>
  );
}


🛡️ 9. Habilitar CORS na sua API .NET
No Program.cs da sua API:
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowAll",
        policy => policy.AllowAnyOrigin()
                        .AllowAnyMethod()
                        .AllowAnyHeader());
});


app.UseCors("AllowAll");


