í ¼í¿—ï¸ Beton-Dysk StacksZbiÃ³r kontenerÃ³w Dockerowych zarzÄ…dzanych w modelu GitOps na serwerze QNAP NAS. System oparty na architekturze No-Ports, gdzie kaÅ¼da usÅ‚uga jest izolowana i dostÄ™pna wyÅ‚Ä…cznie przez Reverse Proxy.í ¼í¾¯ Filozofia "No-Ports"Wszystkie usÅ‚ugi w tym repozytorium przestrzegajÄ… nastÄ™pujÄ…cych zasad:Zero mapowania portÃ³w na hosta: Å»aden kontener (poza Traefikiem i Gluetunem) nie uÅ¼ywa sekcji ports:.Traefik to serce: CaÅ‚y ruch przechodzi przez gÅ‚Ã³wny router, ktÃ³ry zajmuje siÄ™ certyfikatami SSL i routingiem.Dual-Access: UsÅ‚ugi sÄ… dostÄ™pne publicznie przez Cloudflare Tunnel (domena .pl) oraz prywatnie przez Tailscale (domena .ts.net) dziÄ™ki TSDProxy.í ½í³‚ Struktura ProjektuFolderOpisStanaccess/Core infrastruktury (Traefik, TSDProxy, CF Tunnel)âœ… Aktywnymedia/ArrStack (Radarr, Sonarr, qBit) za bramÄ… VPNí ½íº§ WdroÅ¼enieops/NarzÄ™dzia administracyjne (Portainer, Runner)âœ… Aktywnyprojects/Testowe i poboczne aplikacje (np. whoami)âœ… Aktywnyí ½í» ï¸ Standard tworzenia nowej usÅ‚ugiAby dodaÄ‡ nowÄ… aplikacjÄ™, uÅ¼ywamy poniÅ¼szego schematu etykiet (Labels):YAMLlabels:
  - "traefik.enable=true"
  # Routing (Publiczny || Prywatny)
  - "traefik.http.routers.app.rule=Host(`app.${DOMAIN}`) || Host(`app.${TAIL_ID}.ts.net`)"
  # Port na ktÃ³rym SÅUCHA aplikacja wewnÄ…trz kontenera
  - "traefik.http.services.app.loadbalancer.server.port=8080"
  
  # TSDProxy (Rejestracja w Tailscale)
  - "tsdproxy.enable=true"
  - "tsdproxy.name=app"
  - "tsdproxy.target=traefik" # Ruch zawsze leci przez Traefika
  - "tsdproxy.container_port=80"
í ½í´„ GitOps & DeployWdraÅ¼anie zmian odbywa siÄ™ automatycznie poprzez GitHub Actions:Push na branch main.Self-hosted runner na QNAP synchronizuje pliki do /share/Container/stacks/.Automatyczny restart zmienionych kontenerÃ³w.Standard CommitÃ³wStosujemy Conventional Commits:feat(scope): - nowa usÅ‚uga / funkcjafix(scope): - naprawa bÅ‚Ä™du / routinguinfra(scope): - zmiany w rdzeniu (Traefik, DNS)chore(scope): - porzÄ…dki, README, gitignoreí ½í»¡ï¸ BezpieczeÅ„stwoVPN: Kluczowe usÅ‚ugi (Prowlarr, qBittorrent) dziaÅ‚ajÄ… wewnÄ…trz kontenera Gluetun.Izolacja: Wszystkie usÅ‚ugi spiÄ™te sÄ… w wewnÄ™trznej sieci infra_network.Auth: DostÄ™p administracyjny zabezpieczony przez Tailscale ACLs.