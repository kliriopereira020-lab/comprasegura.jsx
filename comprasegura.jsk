import { useState, useEffect } from "react";

const ALIEXPRESS_AFFILIATE = "SEU_LINK_AFILIADO_AQUI";

const SYSTEM_PROMPT = `Você é o assistente do CompraSegura, um app que ajuda pessoas a encontrar produtos baratos e seguros online.

Quando o utilizador pesquisar um produto, responda APENAS em JSON puro sem markdown:

{
  "produto": "nome do produto pesquisado",
  "resumo": "frase curta sobre o produto",
  "resultados": [
    {
      "id": "1",
      "loja": "AliExpress",
      "titulo": "nome específico do produto",
      "preco_min": número em reais,
      "preco_max": número em reais,
      "nota_seguranca": número de 0 a 10,
      "nivel": "SEGURO" | "CUIDADO" | "SUSPEITO",
      "vendedor": "tipo de vendedor ex: Loja Oficial, Vendedor Verificado",
      "envio": "ex: 15 a 30 dias, Envio grátis",
      "motivo": "frase curta porque é seguro ou não",
      "destaque": true ou false (só um produto recebe true - o mais recomendado)
    },
    {
      "id": "2", 
      "loja": "Mercado Livre",
      "titulo": "nome específico do produto",
      "preco_min": número em reais,
      "preco_max": número em reais,
      "nota_seguranca": número de 0 a 10,
      "nivel": "SEGURO" | "CUIDADO" | "SUSPEITO",
      "vendedor": "tipo de vendedor",
      "envio": "ex: 3 a 7 dias, Frete grátis",
      "motivo": "frase curta",
      "destaque": false
    },
    {
      "id": "3",
      "loja": "Amazon",
      "titulo": "nome específico do produto",
      "preco_min": número em reais,
      "preco_max": número em reais,
      "nota_seguranca": número de 0 a 10,
      "nivel": "SEGURO" | "CUIDADO" | "SUSPEITO",
      "vendedor": "tipo de vendedor",
      "envio": "ex: Prime 1 a 2 dias",
      "motivo": "frase curta",
      "destaque": false
    }
  ],
  "dica": "dica de segurança específica para este tipo de produto"
}

Sempre retorna exatamente 3 resultados, um por loja. Preços realistas em reais brasileiros.`;

const nivelCor = {
  SEGURO: { bg: "#00C853", light: "#00C85320", text: "#00C853" },
  CUIDADO: { bg: "#FFB800", light: "#FFB80020", text: "#FFB800" },
  SUSPEITO: { bg: "#FF4444", light: "#FF444420", text: "#FF4444" },
};

const lojaIcon = {
  "AliExpress": "🛒",
  "Mercado Livre": "🛍️",
  "Amazon": "📦",
};

function StarRating({ nota }) {
  const stars = Math.round(nota / 2);
  return (
    <div style={{ display: "flex", gap: 2 }}>
      {[1,2,3,4,5].map(i => (
        <span key={i} style={{ fontSize: 12, color: i <= stars ? "#FFB800" : "#333" }}>★</span>
      ))}
    </div>
  );
}

function ProdutoCard({ item, index, afiliado }) {
  const [vis, setVis] = useState(false);
  const cor = nivelCor[item.nivel] || nivelCor.CUIDADO;

  useEffect(() => {
    const t = setTimeout(() => setVis(true), index * 150);
    return () => clearTimeout(t);
  }, [index]);

  const handleComprar = () => {
    if (item.loja === "AliExpress" && afiliado) {
      window.open(afiliado, "_blank");
    } else if (item.loja === "Mercado Livre") {
      window.open(`https://www.mercadolivre.com.br/busca?q=${encodeURIComponent(item.titulo)}`, "_blank");
    } else if (item.loja === "Amazon") {
      window.open(`https://www.amazon.com.br/s?k=${encodeURIComponent(item.titulo)}`, "_blank");
    }
  };

  return (
    <div style={{
      background: item.destaque ? "#0f1a0f" : "#111",
      border: item.destaque ? "1px solid #00C85344" : "1px solid #1e1e1e",
      borderRadius: 16,
      padding: 20,
      marginBottom: 12,
      opacity: vis ? 1 : 0,
      transform: vis ? "translateY(0)" : "translateY(20px)",
      transition: "all 0.4s ease",
      position: "relative",
      boxShadow: item.destaque ? "0 0 30px #00C85312" : "none"
    }}>
      {item.destaque && (
        <div style={{
          position: "absolute", top: -10, right: 16,
          background: "linear-gradient(135deg, #00C853, #00a846)",
          color: "#fff", fontSize: 10, fontWeight: 800,
          padding: "3px 12px", borderRadius: 99, letterSpacing: 1.5
        }}>
          MAIS RECOMENDADO
        </div>
      )}

      <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", marginBottom: 12 }}>
        <div style={{ flex: 1 }}>
          <div style={{ display: "flex", alignItems: "center", gap: 8, marginBottom: 4 }}>
            <span style={{ fontSize: 18 }}>{lojaIcon[item.loja] || "🛒"}</span>
            <span style={{ color: "#888", fontSize: 12, fontWeight: 600, letterSpacing: 1 }}>{item.loja}</span>
          </div>
          <p style={{ color: "#eee", fontSize: 14, fontWeight: 600, margin: 0, lineHeight: 1.4 }}>
            {item.titulo}
          </p>
        </div>
        <div style={{ textAlign: "right", marginLeft: 12 }}>
          <div style={{ color: "#fff", fontSize: 18, fontWeight: 800 }}>
            R${item.preco_min}
          </div>
          {item.preco_max > item.preco_min && (
            <div style={{ color: "#555", fontSize: 11 }}>até R${item.preco_max}</div>
          )}
        </div>
      </div>

      <div style={{ display: "flex", alignItems: "center", gap: 10, marginBottom: 12 }}>
        <StarRating nota={item.nota_seguranca} />
        <span style={{
          background: cor.light, color: cor.text,
          fontSize: 10, fontWeight: 700, padding: "2px 8px",
          borderRadius: 99, letterSpacing: 1
        }}>
          {item.nivel}
        </span>
        <span style={{ color: "#555", fontSize: 11 }}>{item.nota_seguranca}/10</span>
      </div>

      <div style={{ display: "flex", gap: 8, marginBottom: 12, flexWrap: "wrap" }}>
        <span style={{ background: "#1a1a1a", color: "#888", fontSize: 11, padding: "3px 10px", borderRadius: 99 }}>
          👤 {item.vendedor}
        </span>
        <span style={{ background: "#1a1a1a", color: "#888", fontSize: 11, padding: "3px 10px", borderRadius: 99 }}>
          🚚 {item.envio}
        </span>
      </div>

      <p style={{ color: "#666", fontSize: 12, margin: "0 0 14px", lineHeight: 1.4 }}>
        {item.motivo}
      </p>

      <button
        onClick={handleComprar}
        style={{
          width: "100%", padding: "11px",
          background: item.destaque
            ? "linear-gradient(135deg, #00C853, #00a846)"
            : "#1a1a1a",
          border: item.destaque ? "none" : "1px solid #2a2a2a",
          borderRadius: 10, color: item.destaque ? "#fff" : "#888",
          fontSize: 13, fontWeight: 700, cursor: "pointer",
          fontFamily: "inherit", letterSpacing: 0.5,
          transition: "all 0.2s"
        }}
      >
        {item.destaque ? "✅ Comprar com Segurança" : "Ver Produto"}
      </button>
    </div>
  );
}

export default function CompraSegura() {
  const [query, setQuery] = useState("");
  const [loading, setLoading] = useState(false);
  const [resultado, setResultado] = useState(null);
  const [erro, setErro] = useState("");
  const [dots, setDots] = useState(".");
  const [afiliado] = useState(ALIEXPRESS_AFFILIATE);

  useEffect(() => {
    if (!loading) return;
    const i = setInterval(() => setDots(d => d.length >= 3 ? "." : d + "."), 400);
    return () => clearInterval(i);
  }, [loading]);

  async function pesquisar() {
    if (!query.trim()) return;
    setLoading(true);
    setResultado(null);
    setErro("");

    try {
      const res = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          model: "claude-sonnet-4-20250514",
          max_tokens: 1000,
          system: SYSTEM_PROMPT,
          messages: [{ role: "user", content: `Pesquisa: ${query}` }]
        })
      });

      const data = await res.json();
      const text = data.content?.map(b => b.text || "").join("") || "";
      const clean = text.replace(/```json|```/g, "").trim();
      const parsed = JSON.parse(clean);
      setResultado(parsed);
    } catch (e) {
      setErro("Erro ao pesquisar. Tenta novamente.");
    }
    setLoading(false);
  }

  const populares = ["iPhone 16", "AirPods Pro", "Tênis Nike", "Smart TV 50\"", "Perfume importado"];

  return (
    <div style={{
      minHeight: "100vh",
      background: "#080808",
      fontFamily: "'Syne', sans-serif",
      display: "flex", flexDirection: "column", alignItems: "center",
      padding: "32px 16px 80px",
    }}>
      <link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet" />

      {/* Header */}
      <div style={{ textAlign: "center", marginBottom: 32 }}>
        <div style={{
          display: "inline-flex", alignItems: "center", gap: 10,
          background: "#111", border: "1px solid #1e1e1e",
          borderRadius: 99, padding: "6px 16px", marginBottom: 16
        }}>
          <span style={{ fontSize: 16 }}>🛡️</span>
          <span style={{ color: "#555", fontSize: 11, letterSpacing: 2, fontWeight: 700 }}>COMPRA SEGURA</span>
        </div>
        <h1 style={{
          fontSize: 38, fontWeight: 800, margin: 0, lineHeight: 1.1,
          color: "#fff"
        }}>
          Compra mais barato.<br />
          <span style={{
            background: "linear-gradient(135deg, #00C853, #00e676)",
            WebkitBackgroundClip: "text", WebkitTextFillColor: "transparent"
          }}>Sem levar golpe.</span>
        </h1>
        <p style={{ color: "#444", fontSize: 13, marginTop: 10 }}>
          Comparamos AliExpress, Mercado Livre e Amazon por ti
        </p>
      </div>

      {/* Search */}
      <div style={{ width: "100%", maxWidth: 520, marginBottom: 24 }}>
        <div style={{
          display: "flex", gap: 8,
          background: "#111", border: "1px solid #1e1e1e",
          borderRadius: 14, padding: "8px 8px 8px 16px",
          marginBottom: 12
        }}>
          <input
            value={query}
            onChange={e => setQuery(e.target.value)}
            onKeyDown={e => e.key === "Enter" && pesquisar()}
            placeholder="Ex: iPhone 16, tênis Nike, perfume..."
            style={{
              flex: 1, background: "none", border: "none", outline: "none",
              color: "#eee", fontSize: 15, fontFamily: "inherit"
            }}
          />
          <button
            onClick={pesquisar}
            disabled={loading || !query.trim()}
            style={{
              padding: "10px 20px", borderRadius: 10, border: "none",
              background: loading || !query.trim()
                ? "#1a1a1a"
                : "linear-gradient(135deg, #00C853, #00a846)",
              color: loading || !query.trim() ? "#444" : "#fff",
              fontSize: 13, fontWeight: 700, cursor: loading || !query.trim() ? "not-allowed" : "pointer",
              fontFamily: "inherit", whiteSpace: "nowrap"
            }}
          >
            {loading ? `${dots}` : "Pesquisar"}
          </button>
        </div>

        {/* Populares */}
        {!resultado && (
          <div style={{ display: "flex", gap: 6, flexWrap: "wrap" }}>
            <span style={{ color: "#444", fontSize: 11, alignSelf: "center" }}>Popular:</span>
            {populares.map(p => (
              <button
                key={p}
                onClick={() => { setQuery(p); }}
                style={{
                  background: "#111", border: "1px solid #1e1e1e",
                  borderRadius: 99, color: "#666", fontSize: 11,
                  padding: "4px 12px", cursor: "pointer", fontFamily: "inherit"
                }}
              >
                {p}
              </button>
            ))}
          </div>
        )}
      </div>

      {erro && (
        <p style={{ color: "#FF4444", fontSize: 14, marginBottom: 16 }}>{erro}</p>
      )}

      {/* Loading */}
      {loading && (
        <div style={{ textAlign: "center", padding: 40 }}>
          <div style={{ fontSize: 32, marginBottom: 12 }}>🔍</div>
          <p style={{ color: "#444", fontSize: 14 }}>A comparar preços e segurança{dots}</p>
        </div>
      )}

      {/* Resultados */}
      {resultado && (
        <div style={{ width: "100%", maxWidth: 520 }}>
          <div style={{ marginBottom: 20 }}>
            <h2 style={{ color: "#fff", fontSize: 18, fontWeight: 700, margin: "0 0 4px" }}>
              {resultado.produto}
            </h2>
            <p style={{ color: "#555", fontSize: 13, margin: 0 }}>{resultado.resumo}</p>
          </div>

          {resultado.resultados?.map((item, i) => (
            <ProdutoCard key={item.id} item={item} index={i} afiliado={afiliado} />
          ))}

          {resultado.dica && (
            <div style={{
              background: "#0a0f0a", border: "1px solid #00C85322",
              borderRadius: 14, padding: 16, marginTop: 8
            }}>
              <p style={{ color: "#00C853", fontSize: 12, fontWeight: 700, margin: "0 0 6px", letterSpacing: 1 }}>
                💡 DICA DE SEGURANÇA
              </p>
              <p style={{ color: "#666", fontSize: 13, margin: 0, lineHeight: 1.5 }}>
                {resultado.dica}
              </p>
            </div>
          )}

          <button
            onClick={() => { setResultado(null); setQuery(""); }}
            style={{
              width: "100%", marginTop: 16, padding: "13px",
              background: "none", border: "1px solid #1e1e1e",
              borderRadius: 12, color: "#444", fontSize: 13,
              cursor: "pointer", fontFamily: "inherit"
            }}
          >
            Nova pesquisa
          </button>
        </div>
      )}
    </div>
  );
}
