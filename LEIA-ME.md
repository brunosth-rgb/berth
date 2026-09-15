# Croqui operacional público — atualização

## Arquivos

1. Substitua `index.html` na raiz do repositório do GitHub Pages.
2. Substitua `firestore.rules` no repositório e **publique também seu conteúdo em Firebase Console → Firestore Database → Rules → Publish**. Enviar esse arquivo ao GitHub não atualiza as regras do Firebase.

Os desenhos dos guindastes já estão incorporados ao HTML. Não é necessário substituir assets. A configuração Firebase do projeto `berth-37ab3` foi preservada. Não recrie banco, contas ou lista de usuários autorizados.

## Primeiro uso

Após publicar as regras e atualizar o site, entre com uma conta autorizada e aguarde a sincronização. Abra o croqui desejado em **Abrir / excluir**. Se o editar, salve antes. Clique em **Definir como croqui atual**.

Abra o site em uma janela anônima: o desenho escolhido deve aparecer com data, referência, horário da publicação e indicação de somente leitura. Sem documento publicado, a página apresenta estado vazio e permite entrar.

Salvar ou excluir um croqui privado não altera a cópia pública. Para trocar a situação operacional exibida, use novamente **Definir como croqui atual**. Para retirar a publicação sem substituí-la, o administrador pode excluir somente `public/currentSketch` no console Firebase.

## Dados e acesso

A cópia pública contém data, referência, sentido, dimensões e posições de navios, cores, amarrações, guindastes, cabeços, defensas, limites e parâmetros do cais, versão do formato e horário do servidor. Os identificadores de navios são renumerados. Campos são copiados por uma lista explícita, inclusive dentro dos objetos; UID, e-mail, autor, revisão privada, identificador do croqui original e observações livres não são copiados. Use nomes de navios e referências adequados para visualização pública.

Somente a exceção `public/currentSketch` foi adicionada às regras existentes: leitura pública e escrita por `member()`. As regras privadas anteriores permanecem intactas. O aplicativo só libera edição após o Firestore confirmar o acesso às coleções privadas. O carregamento sem login não recupera a biblioteca ou croquis privados do cache local.

## Validação

Testes de navegador com Firestore simulado passaram: estado vazio, desenho e amarrações, bloqueios antes de autorização, retorno da edição, publicação explícita, remoção de campos administrativos aninhados, salvamento privado sem publicação automática, rejeição de publicação com mudanças não salvas e retorno à visualização pública ao sair. A tela pública também foi inspecionada visualmente.

Esta entrega usa a versão final local da tarefa anterior, com Firebase configurado. Não foi possível confirmar sua igualdade com o site publicado. Nenhuma alteração foi enviada ao GitHub ou ao Firebase nesta tarefa. O teste com contas reais e regras publicadas deve ser feito após a substituição dos arquivos.
