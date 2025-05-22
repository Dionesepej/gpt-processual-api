from fastapi import FastAPI
from pydantic import BaseModel
from typing import List, Optional

app = FastAPI()

# MODELOS

class QuestaoInput(BaseModel):
    tema: str
    estilo: str
    nivel: Optional[str] = "intermediário"

class DiagnosticoInput(BaseModel):
    resposta_aluno: str
    questao_id: Optional[str] = None

class ReflexionInput(BaseModel):
    contexto: str

class SummaryInput(BaseModel):
    texto: str
    nivel: Optional[str] = "intermediário"

class FeedbackInput(BaseModel):
    resposta: str
    desempenho: Optional[str] = None

class MemoriaInput(BaseModel):
    chave: str
    valor: str

class ObserverInput(BaseModel):
    evento: str
    detalhes: Optional[str] = None

# ROTAS

@app.post("/gerar-questao")
def gerar_questao(req: QuestaoInput):
    return {
        "enunciado": f"Sobre o tema '{req.tema}', analise a afirmativa abaixo...",
        "alternativas": [
            "A) Alternativa 1",
            "B) Alternativa 2",
            "C) Alternativa 3",
            "D) Alternativa 4"
        ],
        "gabarito": "B"
    }

@app.post("/diagnostico")
def diagnosticar_resposta(req: DiagnosticoInput):
    return {
        "avaliacao": "Sua resposta apresenta entendimento parcial do tema.",
        "acao_sugerida": "Rever a doutrina aplicada ao tema."
    }

@app.post("/reflexion")
def reflexion(req: ReflexionInput):
    return {
        "resposta_replanejada": f"Plano ajustado: Reforçar abordagem de '{req.contexto}' com esquemas visuais e casos simulados."
    }

@app.post("/summary")
def summary(req: SummaryInput):
    return {
        "resumo": f"Resumo (nível {req.nivel}): {req.texto[:120]}..."
    }

@app.post("/feedback")
def feedback(req: FeedbackInput):
    return {
        "comentario": f"Análise do desempenho: {req.desempenho or 'não informado'}. Sua resposta contém bons argumentos, mas falta fundamentação legal."
    }

@app.post("/memoria")
def memoria(req: MemoriaInput):
    return {
        "status": f"Memória salva: {req.chave} = {req.valor}"
    }

@app.post("/observer")
def observer(req: ObserverInput):
    return {
        "observacao_id": "log-" + req.evento.lower().replace(" ", "-")
    }

@app.get("/")
def healthcheck():
    return {"status": "ok"}
