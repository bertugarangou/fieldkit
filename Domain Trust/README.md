# Domain Trust Issues

## Problema confiança del domini

LSA és el servei de credencials de Windows. Des de la creació de Windows 11 (i també implementat a W10), aquest servei es pot configurar perquè Credential Guard, servei de protecció del servei LSA, no permeti que un executable (binari, driver, llibreria, etc) contacti amb la API de LSA si aquest executable no està firmat per MS (o autirtzat per MS: WHQL certification).

Els problemes semblen passar quan un ordinador està 30 dies fora del domini (apagat o fora del Campus) o quan no tenen TPM (antics d'aules o quan estan fora del domini 30 dies i perden credencials de DCs: Inebitable mentres no tinguem EntraID cloud)
_______________________________
1. En primer lloc de forma passiva es posa el servei Credential Guard a 0 (RunAsPPL = 0) perquè no dongui problemes.  \
	Si se soluciona, ho deixem com a sol·lució temporal.  \
	
2. Quan passi, anar a una aula i amb admin local:  \
	 nltest /sc_verify:<domini>  \
	 whoami /all  \
	 klist  \
	 (buscar errors)   \
	   
	 Get-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Lsa' | Select RunAsPPL (dona 0?)  \
	   
	 tasklist /fi "imagename eq lsass.exe" (agafar PID)  \
	 sigcheck -m <PID> (quins codis o executables apareixen? -> Son els causants!)  \
	 
