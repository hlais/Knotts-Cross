# Knotts-Cross
Unity Source Code - Knotts and Cross 
using System.Collections;
using System.Collections.Generic;
using UnityEngine.UI;
using UnityEngine;

public class GameController : MonoBehaviour
{
	public Text[] buttonList;
	private string playerSide = "X";

	public GameObject gameOverPanel;
	public Text gameOverText;



	private int moveCount;

	public GameObject restartButton; // before this we need to set up an acutull butoon now 



	void Awake()
	{
		gameOverPanel.SetActive(false);
		SetGameControllerReferenceOnButtons();
		moveCount = 0;
		restartButton.SetActive(false);//we refereenced the restartButton. by dragging gameobject into public empty filed
	}

	void SetGameControllerReferenceOnButtons()
	{
		for (int i = 0; i < buttonList.Length; i++)
		{
			buttonList[i].GetComponentInParent<GridSpace>().SetGameControllerReference(this); // on the prefab we created
		}
	}
	public string GetPlayerSide()
	{
		return playerSide;
	}
#region
	public void EndTurn()
	{
		moveCount++;

		if (buttonList[0].text == playerSide && buttonList[1].text == playerSide && buttonList[2].text == playerSide)
		{
			GameOver(playerSide); //playerside is an argument
		}
		if (buttonList[3].text == playerSide && buttonList[4].text == playerSide && buttonList[5].text == playerSide)
		{
			GameOver(playerSide);
		}
		if (buttonList[6].text == playerSide && buttonList[7].text == playerSide && buttonList[8].text == playerSide)
		{
			GameOver(playerSide);
		}
		if (buttonList[0].text == playerSide && buttonList[4].text == playerSide && buttonList[8].text == playerSide)
		{
			GameOver(playerSide);
		}
		if (buttonList[2].text == playerSide && buttonList[4].text == playerSide && buttonList[6].text == playerSide)
		{
			GameOver(playerSide);
		}
		if (buttonList[0].text == playerSide && buttonList[3].text == playerSide && buttonList[6].text == playerSide)
		{
            GameOver(playerSide);
		}
		if (buttonList[1].text == playerSide && buttonList[4].text == playerSide && buttonList[7].text == playerSide)
		{
			GameOver(playerSide);
		}
		if (buttonList[2].text == playerSide && buttonList[5].text == playerSide && buttonList[8].text == playerSide)
		{
			GameOver(playerSide);
		}

		if (moveCount >= 9)
		{
			GameOver("draw");
			//SetGameOverText( "It's a draw!");//
		}
		ChangeSides();
	}
#endregion
	void GameOver(string winningPlayer)
	{ /*
		for (int i = 0; i<buttonList.Length; i++)
		{
			buttonList[i].GetComponentInParent<Button>().interactable = false;
		} */
		SetBoardInteractable(false);

		if (winningPlayer == "draw")
		{
			SetGameOverText("Its a Draw!");
		}
		else
		{
			SetGameOverText(winningPlayer+ " Wins!");
		}
		restartButton.SetActive(true);
	}
	void ChangeSides()
	{
		playerSide = (playerSide == "X")? "O" : "X";
	}
	void SetGameOverText(string value)
	{
		gameOverPanel.SetActive(true);
		gameOverText.text = value;
	}

	public void RestartGame()
	{
		playerSide = "X";
		moveCount = 0;
		gameOverPanel.SetActive (false); // SetActive actives or Deactives gameObject. THis is a unity thang
		SetBoardInteractable(true);

		for (int i = 0; i < buttonList.Length; i++) // this was copied from the gameOver 
		{
			//buttonList[i].GetComponentInParent<Button>().interactable = true; // will replace this

			buttonList[i].text = "";
		}
		restartButton.SetActive(false);
	}
	void SetBoardInteractable(bool toggle)
	{ 
		for (int i = 0; i<buttonList.Length; i++)
		{
			buttonList[i].GetComponentInParent<Button>().interactable = toggle;
		}
	}
}
	public class GridSpace : MonoBehaviour
{

	public Button button;
	public Text buttonText;


	private GameController gameController;

	public void SetSpace()
	{
		buttonText.text = gameController.GetPlayerSide();
		button.interactable = false;
		gameController.EndTurn();
	}

	public void SetGameControllerReference(GameController controller)

	{
		gameController = controller;
	}

}
