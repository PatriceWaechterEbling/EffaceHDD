# EffaceHDD
Montre comment effacer et reimager un HDD en mode Terminal avec DISM

modification dans les pipes  
void ExecuteCommandeDos(LPSTR Commande) { //mise a jour dans le pipe

	SECURITY_ATTRIBUTES secattr;
	ZeroMemory(&secattr, sizeof(secattr));
	secattr.nLength = sizeof(secattr);
	secattr.bInheritHandle = TRUE;
	HANDLE rPipe, wPipe;
	CreatePipe(&rPipe, &wPipe, &secattr, 0);
	STARTUPINFO sInfo;
	ZeroMemory(&sInfo, sizeof(sInfo));
	PROCESS_INFORMATION pInfo;
	ZeroMemory(&pInfo, sizeof(pInfo));
	sInfo.cb = sizeof(sInfo);
	sInfo.dwFlags = STARTF_USESTDHANDLES;
	sInfo.hStdInput = NULL;
	sInfo.hStdOutput = wPipe;
	sInfo.hStdError = wPipe;
	CreateProcess(0, (LPSTR)Commande, 0, 0, TRUE, NORMAL_PRIORITY_CLASS | CREATE_FORCEDOS/*CREATE_NO_WINDOW*/, 0, 0, &sInfo, &pInfo);
	CloseHandle(wPipe);
	char buf[4096]{};//4ko de memoire
	DWORD reDword;
	BOOL res;
	do {
		res = ::ReadFile(rPipe, buf,4096, &reDword, 0);
		strcat(m_csOutput, buf);
	} while (res);
	SetDlgItemText(hWnd, IDC_EDIT1, m_csOutput);
}
