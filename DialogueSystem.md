# Dialogue-Scripts
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;

public struct DialogueUIOption
{
	public DialogueOption line;
	public UnityAction[] onclickEvent;

	int[] numbers;
}


public class DialogueSystem : MonoBehaviour {

	public DialogueDatabase database;

	AudioSource source;
	Dictionary<string, DialogueOption> dialogueTable = new Dictionary<string, DialogueOption> ();

	DialogueOption m_currentLine = null;



	void Awake()
	{
		LoadDatabase (database);
		source = GetComponent<AudioSource> ();
	}

	void LoadDatabase(DialogueDatabase newDatabase)
	{
		database = newDatabase;

		foreach (DialogueOption option in database.dialogueLines) 
		{
			dialogueTable.Add (option.name, option);
		}
	} 
	void StartLine (DialogueOption option)
	{
		
		if (option != null)
		{
			m_currentLine = option;

			source.PlayOneShot(m_currentLine.lineAudio);

			DialogueUIOption data = new DialogueUIOption ();
			data.line = option;
			data.onclickEvent = new UnityAction[option.responses.Length];

			int index = 0;
			foreach (DialogueOption response in option.responses) {
				int currentIndex = index;
				data.onclickEvent [index] += () => {
					this.NextLine (currentIndex);
				};
				index++;
			}
			if (GameEvents.onStartDialogue != null) 
			{
				GameEvents.onStartDialogue (data);
			}
		}
	}

	public void StartLine (string dialogueLineName)
	{
		DialogueOption option = null;
		dialogueTable.TryGetValue (dialogueLineName, out option);

		StartLine (option);

	}
	public void NextLine(int lineIndex)
	{
		Debug.Log (string.Format ("Next Line Called: {0}", lineIndex));
		if(lineIndex < m_currentLine.responses.Length)
		{
		StartLine (m_currentLine.responses [lineIndex]);
		}
	}
}
