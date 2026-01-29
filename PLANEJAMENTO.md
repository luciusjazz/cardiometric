## Plano de próximos passos – ECG Logger (PWA)

### Estado atual (v32)
- Conexão Polar H10 (HR + PMD/ECG) e coleta de ECG/FC/RR: OK
- Congelamento com histórico completo durante a coleta: OK (rolagem no histórico)
- Revisão: pan/zoom (mouse, touch/pinch e drag-to-zoom), navegação com barra e atalhos, lista de marcadores clicáveis: OK
- Exportações: CSV ECG e RR com coluna `marker`; PDF com escala estável, timestamps, labels de marcadores nas tiras e notas de rodapé; índice de marcadores no início: OK
- Filtros: em tempo real (toggle) e na revisão (UI) aplicados: OK
- UI de métricas: FC atual adicionada, colunas e timer de sessão: OK
- Sessões: exportar/importar arquivo `.json` da sessão; opção IndexedDB básica (não exposta na UI): OK
- PWA: manifest + service worker; app funciona offline e pode ser instalada em HTTPS: OK

---

### Backlog priorizado (com critérios de aceite)

1) PWA & deploy (média prioridade)  
- Objetivo: finalizar o pacote de distribuição e garantir atualização controlada.  
- Entregas:
  - Ícones (192 px/512 px), splash image e página de fallback offline.
  - Estratégia de versionamento do service worker (limpeza/renovação de cache + aviso de nova versão).
  - Repositório GitHub com README e publicação na Vercel (HTTPS) apontando para `ecg_v33.html`; testes de instalação e Web Bluetooth em produção.
- Aceite: aplicativo instalável e funcional offline em domínio seguro, com atualização previsível.

2) Sessões avançadas / sincronização (média prioridade)  
- Objetivo: evoluir além do gerenciador local.  
- Entregas:
  - File System Access (quando suportado) para definir pasta padrão e salvar/restaurar sem prompts.
  - Plano/POC para sincronizar sessões em nuvem (Supabase/Firebase etc.), preservando o modo “guest”.
- Aceite: usuário organiza sessões locais com facilidade e existe caminho claro para backup remoto.

3) Compatibilidade e amostragem (baixa/média prioridade)  
- Objetivo: suportar sensores apenas RR e documentar a taxa real do H10.  
- Entregas:
  - Fallback “RR only” quando o PMD/ECG não estiver disponível, com exportações coerentes.
  - Medição/ajuste automático da frequência do H10 (130 vs 170 Hz) e indicação na UI/exportações.
- Aceite: aplicativo continua útil com sensores somente de frequência cardíaca, preservando integridade temporal.

4) Testes e validação (contínuo)  
- Roteiros:
  - Conexão/reconexão BLE e sessões >30 min (performance, autosave, recuperação).
  - UX mobile (pinch, botões +/−, fullscreen) em diferentes aparelhos, inclusive iOS (modo leitura).
  - Exportações CSV/PDF com diferentes configurações e marcadores.
  - Fluxo PWA em produção: instalação/desinstalação, fallback offline e atualização pós-deploy.

### Notas de implementação
- Arquivo principal: `ecg_v31.html` (UI, lógica de coleta, gráficos).
- Novos arquivos: `manifest.json`, `service-worker.js`.
- Bibliotecas: Chart.js + chartjs-plugin-zoom; jsPDF.
- Polar H10: UUIDs de HR e PMD já configurados.

### Riscos e mitigação
- Desempenho com séries longas: paginar/segmentar revisão se necessário.
- Permissões Bluetooth no navegador: mensagens claras de erro e dicas.
- Cache PWA desatualizado: versão de cache e estratégia de atualização controlada.

### Quando voltar (ordem sugerida de execução)
1. Preparar deploy (ícones, fallback, service-worker versionado) e publicar no GitHub/Vercel garantindo HTTPS e testes em dispositivos reais.
2. Planejar/implementar File System Access e definir abordagem de sincronização em nuvem para sessões.
3. Implementar fallback “RR only” e ajuste automático da taxa do H10.
4. Rodar o pacote de testes (BLE longo, mobile UX, exportações, fluxo PWA) antes do próximo ciclo.

