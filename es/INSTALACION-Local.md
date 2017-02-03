
Instalacion de Datal en equipo (Debian - Ubuntu)
================================================

1. Instalar git y curl
2. Crear un usuario llamado datal
3. su datal
4. cd ~
5. git clone https://github.com/datal-org/datal.git app
6. cd /home/datal/app
7. git submodule init
8. git submodule update
9. exit
10. curl -L https://bootstrap.saltstack.com | bash /dev/stdin
11. Reemplazar el /etc/salt/minion con

    ```
    file_client: local
    
    file_roots:
      base:
        - /home/datal/app/salt/roots/salt
        - /home/datal/app/salt/roots/formulas/nginx-formula
    
    
    
    pillar_roots:
      base:
        - /home/datal/app/salt/roots/pillar
    ```

12. Reiniciar el servicio salt-minion
13. Ejecutar el provision: salt-call state.highstate
