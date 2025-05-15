import React, { useState } from 'react';
import { Button } from '@/components/ui/button';
import { Card, CardContent } from '@/components/ui/card';

const winningCombinations = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6],
];

export default function TicTacToeApp() {
  const [board, setBoard] = useState(Array(9).fill(null));
  const [isXNext, setIsXNext] = useState(true);
  const [winner, setWinner] = useState(null);

  const handleClick = (index) => {
    if (board[index] || winner) return;
    const newBoard = [...board];
    newBoard[index] = isXNext ? 'X' : 'O';
    setBoard(newBoard);
    setIsXNext(!isXNext);
    checkWinner(newBoard);
  };

  const checkWinner = (board) => {
    for (let combo of winningCombinations) {
      const [a, b, c] = combo;
      if (board[a] && board[a] === board[b] && board[a] === board[c]) {
        setWinner(board[a]);
        return;
      }
    }
    if (!board.includes(null)) {
      setWinner('Draw');
    }
  };

  const resetGame = () => {
    setBoard(Array(9).fill(null));
    setIsXNext(true);
    setWinner(null);
  };

  return (
    <div className="p-6 max-w-md mx-auto text-center">
      <Card className="shadow-xl">
        <CardContent>
          <h1 className="text-2xl font-bold mb-4">Tic Tac Toe</h1>
          <div className="grid grid-cols-3 gap-2 w-48 mx-auto mb-4">
            {board.map((cell, index) => (
              <Button
                key={index}
                className="h-16 text-xl font-bold"
                onClick={() => handleClick(index)}
              >
                {cell}
              </Button>
            ))}
          </div>
          <div className="mb-4">
            {winner ? (
              <p className="text-xl font-semibold">
                {winner === 'Draw' ? 'It\'s a Draw!' : `${winner} Wins!`}
              </p>
            ) : (
              <p className="text-lg">Next Player: {isXNext ? 'X' : 'O'}</p>
            )}
          </div>
          <Button onClick={resetGame}>Reset Game</Button>
        </CardContent>
      </Card>
    </div>
  );
}
