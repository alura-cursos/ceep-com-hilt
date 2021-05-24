## Funcionalidades do projeto

O App lista notas com imagem, título e descrição, sejam armazenadas internamente ou recebidas via web API.

![](img/amostra.gif)

## Técnicas e tecnologias no projeto

As técnicas e tecnologias utilizadas pra isso são:

- **Navigation**: configuração da navegação das telas
- **RecyclerView**: listagem das notas
- **ConstraintLayout**: ViewGroup para implementar o layout das notas
- **Coil**: carregar imagens via requisição HTTP
- **Retrofit**: requisição HTTP para buscar novas notas
- **Room**: Armazenamento interno das notas
- **ViewModel**: modelo para manter regra de negócio das Activities/Fragments
- **Coroutines**: processamento assíncrono
- **View Binding**: busca de views do layout de forma segura

### Observações da Web API

As notas apresentadas na amostra são carregadas por meio de uma web API mockada, ou seja, para que você consiga fazer a mesma simulação, você precisa utilizar um endereço de uma API em funcionamento. 

Caso você implemente a sua própria ou utilize uma mockada, é necessário configurar um end-point GET para `/notes`. Como corpo da resposta, você deve devolver uma lista de notas:

```json
[
    {
        "id" : 1,
        "title": "my first title",
        "description": "my first description"
    },
    {
        "id" : 2,
        "title": "my second title",
        "description": "my second description"
    }
]
```

O App também funciona sem essa integração com uma Web API, o grande detalhe é que ele vai logar no logcat que não conseguiu fazer uma comunicação:

```
br.com.alura.ceep E/NoteRepository: findAll: failure to fetch new notes from API
```

Caso queira adicionar notas sem a conexão com a Web API, você pode acessar a `CeepApp` e descomentar o código do `onCreate()`. Na amostra de código é apresentado um exemplo de código para salvar notas:

```kotlin
class CeepApp : Application() {

    override fun onCreate() {
        super.onCreate()

        saveNotes(
            listOf(
                Note(
                    title = "first title",
                    description = "first description"
                ),
                Note(
                    title = "second title",
                    description = "second description"
                )
            )
        )
    }

    private fun saveNotes(notes: List<Note>) {
        CoroutineScope(IO).launch {
            AppDatabase.getInstance(this@CeepApp)
                .getNoteDao().save(notes)
        }
    }

}
```

Neste exemplo, salvam 2 notas e você pode modificar a quantidade conforme a sua preferência. A única orientação é que depois que executar o App uma vez, comente ou apague essa amostra de código para que você consiga testar as mudanças e não apareça várias notas!

## Acesso ao projeto e código fonte

Você pode acessar o projeto em seus seguintes estados:

- inicial: sem a integração com o Hilt:
    - [baixar](https://github.com/alura-cursos/ceep-com-hilt/archive/refs/heads/initial-project.zip)
    - [código fonte](https://github.com/alura-cursos/ceep-com-hilt/tree/initial-project)
- final: com a integração com o Hilt:
    - [baixar](https://github.com/alura-cursos/ceep-com-hilt/archive/refs/heads/final.zip)
    - [código fonte](https://github.com/alura-cursos/ceep-com-hilt/tree/final)
