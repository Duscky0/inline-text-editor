🧩 Inline Text Editor Bookmarklet

Transforme qualquer texto selecionado em um campo editável direto na página.
Permite modificar visualmente o conteúdo sem recarregar o site.


---

🚀 Como Usar

1. Copie o código abaixo.


2. Crie um novo favorito no navegador.


3. Cole o código no campo de URL.


4. Em qualquer site, selecione um texto e clique no favorito.


5. Edite o texto na caixa exibida e clique em Salvar.


6. As alterações são apenas visuais (sumirão ao recarregar a página).




---

💻 Bookmarklet

> 🔒 Dica: o código abaixo deve ser copiado inteiro para o campo de URL do favorito.



```js
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
```

