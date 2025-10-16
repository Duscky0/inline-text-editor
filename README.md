🧩 Descrição de Uso

Bookmarklet que transforma o texto selecionado em um campo editável direto na página.
Permite editar e visualizar a mudança imediatamente, sem recarregar o site.

Passos

1. Selecione qualquer texto na página.


2. Clique no bookmarklet salvo.


3. Uma caixa aparecerá com o texto selecionado.


4. Edite o conteúdo e clique Salvar.


5. O texto na página será alterado visualmente.


6. Recarregar a página reverte as mudanças.



📜 Código do Bookmarklet

javascript:(function(){
  const sel = window.getSelection();
  if(!sel || sel.rangeCount===0 || sel.isCollapsed){
    alert("Selecione um texto antes de ativar o bookmarklet.");
    return;
  }
  const range = sel.getRangeAt(0).cloneRange();
  const originalText = sel.toString();

  const modal = document.createElement('div');
  Object.assign(modal.style,{
    position:'fixed', left:'50%', top:'50%', transform:'translate(-50%,-50%)',
    zIndex:2147483647, background:'#111', color:'#fff', padding:'12px', borderRadius:'8px',
    boxShadow:'0 6px 24px rgba(0,0,0,.6)', fontFamily:'sans-serif', width:'min(90vw,600px)'
  });

  const ta = document.createElement('textarea');
  ta.value = originalText;
  Object.assign(ta.style,{width:'100%',height:'120px',marginBottom:'8px',padding:'8px',boxSizing:'border-box',fontSize:'14px'});

  const btnSave = document.createElement('button');
  btnSave.textContent = 'Salvar';
  Object.assign(btnSave.style,{marginRight:'8px',padding:'6px 10px'});

  const btnCancel = document.createElement('button');
  btnCancel.textContent = 'Cancelar';
  Object.assign(btnCancel.style,{padding:'6px 10px'});

  const help = document.createElement('div');
  help.textContent = 'Edite o texto e clique Salvar. Recarregue a página para reverter.';
  Object.assign(help.style,{fontSize:'12px',color:'#bbb',marginTop:'8px'});

  modal.appendChild(ta);
  modal.appendChild(btnSave);
  modal.appendChild(btnCancel);
  modal.appendChild(help);
  document.body.appendChild(modal);
  ta.focus();
  ta.select();

  function applyEdited(text){
    try{
      const span = document.createElement('span');
      span.textContent = text;
      span.setAttribute('data-bookmarklet-edit','true');
      Object.assign(span.style,{
        background: 'linear-gradient(90deg, rgba(255,235,59,0.15), rgba(255,235,59,0.05))',
        border: '1px dashed rgba(255,235,59,0.35)',
        padding: '0 2px',
        borderRadius: '2px'
      });
      sel.removeAllRanges();
      sel.addRange(range);
      range.deleteContents();
      range.insertNode(span);
    }catch(e){
      alert('Erro: '+e.message);
    }
  }

  btnSave.onclick = ()=>{ applyEdited(ta.value); modal.remove(); };
  btnCancel.onclick = ()=>{ modal.remove(); };
})();
