void CMiscHacks::FakeWalk(CUserCmd* pCmd, bool &bSendPacket)
{
	IClientEntity* pLocal = hackManager.pLocal();
 
	int FakeWalkKey = Menu::Window.MiscTab.FakeWalk.GetKey();
	if (FakeWalkKey > 0 && GUI.GetKeyState(FakeWalkKey))
	{
		static int iChoked = -1;
	iChoked++;
 
	if (iChoked < 3)
	{
		bSendPacket = false;
		pCmd->tick_count += 10;
		pCmd += 7 + pCmd->tick_count % 2 ? 0 : 1;
		pCmd->buttons |= pLocal->GetMoveType() == IN_BACK;
		pCmd->forwardmove = pCmd->sidemove = 0.f;
	}
	else
	{
		bSendPacket = true;
		iChoked = -1;
		Interfaces::Globals->frametime *= (pLocal->GetVelocity().Length2D()) / 1.f;
		pCmd->buttons |= pLocal->GetMoveType() == IN_FORWARD;
	}
 
	}
}
