using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class DialogueUI : MonoBehaviour {

    public GameObject buttonPrefab;

    private void OnEnable()
    {
        GH.EventSystem.instance.AddListener<DialogueUIOption>(ShowUI);
    }

    private void OnDisable()
    {
        GH.EventSystem.instance.RemoveListener<DialogueUIOption>(ShowUI);
    }

    //SHOWS DIALOGUE OPTION
    void ShowUI(DialogueUIOption option)
    {
        int index = 0;
        foreach(DialogueOption choice in option.line.repsonses)
        {
            GameObject button = GameObject.Instantiate(buttonPrefab, gameObject.transform);

            Button uiButton = button.GetComponent<Button>();
            TMPro.TextMeshProUGUI uiText = button.GetComponentInChildren<TMPro.TextMeshProUGUI>();
            uiText.SetText(choice.line);

            uiButton.onClick.AddListener(option.onClickEvent[index]);
            index++;
        }
    }
}
