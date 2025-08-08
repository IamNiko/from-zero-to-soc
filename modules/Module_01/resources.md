# Módulo 1 — Recursos

## Objetivo del módulo
Reunir enlaces, documentación y material adicional para reforzar lo aprendido en el laboratorio de captura de tráfico base con `tcpdump`.

---

## Documentación oficial

- **tcpdump**
  - Sitio oficial: [https://www.tcpdump.org/](https://www.tcpdump.org/)
  - Página de manual (`man tcpdump`): [https://www.tcpdump.org/manpages/tcpdump.1.html](https://www.tcpdump.org/manpages/tcpdump.1.html)

- **capinfos** (incluido en Wireshark)
  - Página de manual (`man capinfos`): [https://www.wireshark.org/docs/man-pages/capinfos.html](https://www.wireshark.org/docs/man-pages/capinfos.html)

---

## Lecturas recomendadas

- **Wireshark Display Filters** (para análisis posterior de PCAPs):  
  [https://wiki.wireshark.org/DisplayFilters](https://wiki.wireshark.org/DisplayFilters)

- **Expresiones de captura en tcpdump**:  
  [https://danielmiessler.com/study/tcpdump/](https://danielmiessler.com/study/tcpdump/)

---

## Vídeos y tutoriales

- *TCPDump Beginner Tutorial* — 14 min — [YouTube](https://www.youtube.com/watch?v=HkZbzFJjHXI)
- *Wireshark Basics for Network Analysis* — 23 min — [YouTube](https://www.youtube.com/watch?v=TkCSr30UojM)

---

## Prácticas adicionales

1. Capturar solo tráfico HTTP:
   ```bash
   sudo tcpdump -i eth0 port 80 -w http_traffic.pcap

2 Capturar trafico de un host especifico

	sudo tcpdump -i eth0 host 8.8.8.8 -w google_dns.pcap

3 Capturar solo cabeceras (no payload)

	sudo tcpdump -i eth0 -s 96 -w headers_only.pcap

Archivos de ejemplo

    PCAP de práctica de Wireshark: https://wiki.wireshark.org/SampleCaptures
