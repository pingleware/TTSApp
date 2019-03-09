# TTSApp

This is a [sample text-to-speech application](https://msdn.microsoft.com/en-us/library/ee125104%28v=vs.85%29.aspx) from the [Microsoft SAPI 5 SDK](https://msdn.microsoft.com/en-us/library/ee125102%28v=vs.85%29.aspx).

## How it Works?

The heavy lifting is performed by the SAPI or [Microsoft Speech Engine](https://docs.microsoft.com/en-us/previous-versions/office/developer/speech-technologies/hh361572%28v%3doffice.14%29)

A Viseme to Image mapping list is created,

	const int           g_aMapVisemeToImage[22] = { 0,  // SP_VISEME_0 = 0,    // Silence
	11, // SP_VISEME_1,        // AE, AX, AH
	11, // SP_VISEME_2,        // AA
	11, // SP_VISEME_3,        // AO
	10, // SP_VISEME_4,        // EY, EH, UH
	11, // SP_VISEME_5,        // ER
	9,  // SP_VISEME_6,        // y, IY, IH, IX
	2,  // SP_VISEME_7,        // w, UW
	13, // SP_VISEME_8,        // OW
	9,  // SP_VISEME_9,        // AW
	12, // SP_VISEME_10,       // OY
	11, // SP_VISEME_11,       // AY
	9,  // SP_VISEME_12,       // h
	3,  // SP_VISEME_13,       // r
	6,  // SP_VISEME_14,       // l
	7,  // SP_VISEME_15,       // s, z
	8,  // SP_VISEME_16,       // SH, CH, JH, ZH
	5,  // SP_VISEME_17,       // TH, DH
	4,  // SP_VISEME_18,       // f, v
	7,  // SP_VISEME_19,       // d, t, n
	9,  // SP_VISEME_20,       // k, g, NG
	1 };// SP_VISEME_21,       // p, b, m

the bitmaps (images) are loaded,

		hBmp = LoadBitmap(m_hInst, MAKEINTRESOURCE(IDB_MICEYESCLO));
		ImageList_AddMasked(hListBmp, hBmp, 0xff00ff);
		DeleteObject(hBmp);

		ImageList_SetOverlayImage(hListBmp, 1, 1);
		...


As the [ISpVoice::Speak](https://documentation.help/SAPI-5/ISpVoice_Speak.htm) routing is invoke,

	m_cpVoice->Speak(szWTextString, SPF_ASYNC | ((IsDlgButtonChecked(m_hWnd, IDC_SPEAKXML)) ? SPF_IS_XML : SPF_IS_NOT_XML), 0);

the iSpVoice will send the event, [SPEI_VISEME](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/ee450698(v=vs.85)) indicating the Viseme, and get the appropiate bitmap from the list,

			g_iBmp = g_aMapVisemeToImage[event.Viseme()]; // current viseme





