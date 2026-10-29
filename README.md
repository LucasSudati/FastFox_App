# FastFox App — Plano de desenvolvimento experimental

> **Status em 29/10/2026:** planejamento. Nenhuma integração, compressão ou validação descrita abaixo está concluída. Esta documentação pertence à branch `dev/test-telemetry`; a `main` e as releases existentes permanecem inalteradas.

## Contexto e separação dos projetos

- **FastFox** (repositório privado): código-fonte do aplicativo/add-on executado no Assetto Corsa; a integração da telemetria será desenvolvida e testada nele.
- **FastFox_App** (repositório público): site HTML de apresentação e distribuição, com ZIP nas releases e instalador em Python no pacote distribuído, conforme informado pelo desenvolvedor.
- **Datalogger do servidor:** versão mais recente ainda será disponibilizada para inspeção. O formato real dos logs, sua frequência de coleta, o tamanho diário (MB ou Mb) e a forma de transmissão **ainda precisam ser confirmados**.

## Objetivo

Integrar a telemetria do FastFox com os registros do datalogger do servidor, permitindo análise histórica de sessões/corridas, comparação de voltas e investigação de eventos suspeitos. Reduzir o volume armazenado por compressão, sem perder dados necessários para reconstrução e auditoria.

## Plano por etapas

### 1. Levantamento técnico — pendente
- Obter a versão atualizada do datalogger e exemplos reais de logs, incluindo sessões curtas e longas.
- Identificar formatos, campos, unidades, timestamps, taxa de amostragem, identificadores de sessão/carro/piloto, versão do protocolo e tamanho em **bytes**.
- Mapear a telemetria existente no FastFox e o fluxo do instalador/release; registrar diferenças e dependências.
- Criar amostras de teste sem divulgar identificadores pessoais ou dados sensíveis.

### 2. Contrato de dados e compatibilidade — pendente
- Definir um esquema versionado com metadados de sessão, origem, unidades, relógio, amostras e eventos.
- Projetar um adaptador para os logs atuais do servidor e outro para a telemetria existente no FastFox.
- Garantir leitura de arquivos antigos; documentar migrações e comportamento diante de campos ausentes.
- Distinguir dados registrados diretamente de métricas derivadas e sinalizar lacunas na captura.

### 3. Compressão e integridade — pendente
- Medir o volume real antes de escolher a implementação.
- Comparar o formato atual com serialização compacta e compressão **Zstandard (Zstd)**; avaliar alternativas quando houver restrições de runtime.
- Testar compressão por blocos/sessão para permitir leitura parcial e evitar descompressão de arquivos enormes.
- Incluir versão do formato, checksum, tamanho original e tratamento de arquivos incompletos.
- Medir taxa de compressão, tempo de escrita/leitura, CPU, RAM e recuperação após interrupções.
- **Não aplicar compressão com perda** aos dados exigidos para auditoria sem decisão explícita.

### 4. Integração com o FastFox — pendente
- Implementar leitor/adaptador dos arquivos do servidor no código privado do app.
- Reutilizar componentes de visualização e comparação de telemetria quando compatíveis.
- Suportar seleção de sessão, piloto, carro, volta, linha do tempo e eventos disponíveis no log.
- Tratar erros de versão, arquivos corrompidos, dados incompletos e sessões grandes.

### 5. Análise de integridade de corrida — pendente
- Criar indicadores para investigar acelerações, velocidade, posição e transições físicas suspeitas.
- Considerar latência, colisões, teleporte administrativo, física do jogo e características de cada veículo.
- Tratar alertas como **indícios para revisão**, não como prova automática de trapaça.
- Definir retenção, acesso e privacidade dos registros.

### 6. Testes e publicação — pendente
- Validar equivalência entre dados originais e descomprimidos, inclusive em arquivos grandes.
- Comparar telemetria ao vivo e histórica em sessões reais e testar regressões do FastFox.
- Confirmar instalação/atualização pelo instalador Python e compatibilidade com o Assetto Corsa.
- Somente depois publicar um novo ZIP de release e atualizar os links/instruções do site público.

## Critérios de aceite

- Os registros antigos definidos como suportados continuam legíveis.
- Dados descomprimidos preservam integralmente os campos exigidos para análise.
- Há medições reproduzíveis de armazenamento, desempenho e integridade.
- O FastFox consegue carregar e visualizar uma sessão real do servidor.
- A release não substitui a versão estável antes da aprovação dos testes.

## Próximos insumos necessários

1. Código/ZIP atualizado do datalogger do servidor.
2. Amostra real dos arquivos gravados e estimativa confirmada de volume diário.
3. Código atual do módulo de telemetria do FastFox privado, para mapear o contrato de dados.

## Registro de andamento

| Data | Situação | Observação |
| --- | --- | --- |
| 2026-10-29 | Planejamento | Branch experimental criada e roteiro documentado; nenhuma implementação declarada. |
