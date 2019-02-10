# Dialogue-Scripts
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class DialogueUI : MonoBehaviour {

	public GameObject buttonPrefab;

	private void OnEnable()
	{
		GameEvents.onStartDialogue += ShowUI;

	}
		
	private void OnDisable()
	{
		GameEvents.onStartDialogue -= ShowUI;
	}

	public void ShowUI(DialogueUIOption option)
	{
		int index = 0;
		foreach (DialogueOption choice in option.line.responses) 
		{
			GameObject button = GameObject.Instantiate (buttonPrefab, gameObject.transform);

			Button uiButton = button.GetComponent<Button> ();
			TMPro.TextMeshProUGUI uiText = button.GetComponentInChildren<TMPro.TextMeshProUGUI> ();
			uiText.SetText (choice.line);

			uiButton.onClick.AddListener(option.onclickEvent[index]);
			index++;
		}
	}
}
