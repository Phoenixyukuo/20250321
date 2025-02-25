import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { motion } from "framer-motion";

export default function Raffle() {
  const [participants, setParticipants] = useState([]);
  const [name, setName] = useState("");
  const [winners, setWinners] = useState([]);
  const [bonusWinners, setBonusWinners] = useState([]);
  const [prizes, setPrizes] = useState(Array(10).fill(""));
  const [bonusPrizes, setBonusPrizes] = useState(Array(3).fill(""));

  const addParticipant = () => {
    if (name.trim() && !participants.includes(name) && participants.length < 20) {
      setParticipants([...participants, name.trim()]);
      setName("");
    }
  };

  const drawWinners = () => {
    if (participants.length > 0) {
      let selectedWinners = [];
      let availableParticipants = [...participants];
      for (let i = 0; i < 10 && availableParticipants.length > 0; i++) {
        const randomIndex = Math.floor(Math.random() * availableParticipants.length);
        selectedWinners.push({ name: availableParticipants[randomIndex], prize: prizes[i] });
        availableParticipants.splice(randomIndex, 1);
      }
      setWinners(selectedWinners);
    }
  };

  const drawBonusWinners = () => {
    if (participants.length > 0) {
      let selectedBonusWinners = [];
      let availableParticipants = [...participants];
      for (let i = 0; i < 3 && availableParticipants.length > 0; i++) {
        const randomIndex = Math.floor(Math.random() * availableParticipants.length);
        selectedBonusWinners.push({ name: availableParticipants[randomIndex], prize: bonusPrizes[i] });
        availableParticipants.splice(randomIndex, 1);
      }
      setBonusWinners(selectedBonusWinners);
    }
  };

  return (
    <div className="flex flex-col items-center p-6 space-y-6 bg-gradient-to-b from-blue-100 to-blue-300 min-h-screen w-full max-w-sm mx-auto text-center">
      <img src="https://drive.google.com/uc?export=view&id=1N1qO-OmyFcratmK5okD4OQeeFxVoLj3o" alt="TMU OGE Logo" className="w-24 mb-4" />
      <h1 className="text-2xl font-bold text-blue-900">TMU OGE 公開抽獎</h1>
      <div className="flex flex-col w-full space-y-2">
        <Input
          value={name}
          onChange={(e) => setName(e.target.value)}
          placeholder="請輸入您的姓名"
          className="border border-blue-500 p-2 rounded-lg w-full text-center"
        />
        <Button onClick={addParticipant} disabled={participants.length >= 20} className="bg-blue-600 hover:bg-blue-700 text-white p-2 rounded-lg w-full">
          加入抽獎
        </Button>
      </div>
      <Card className="w-full shadow-lg border border-gray-300 bg-white">
        <CardContent className="p-4">
          <h2 className="text-lg font-semibold text-gray-700">設定獎項</h2>
          {prizes.map((prize, index) => (
            <Input
              key={index}
              value={prizes[index]}
              onChange={(e) => {
                let newPrizes = [...prizes];
                newPrizes[index] = e.target.value;
                setPrizes(newPrizes);
              }}
              placeholder={`主獎 ${index + 1} 名稱`}
              className="border border-gray-400 p-2 rounded-lg w-full mt-1"
            />
          ))}
          <h2 className="text-lg font-semibold text-gray-700 mt-4">設定加碼獎</h2>
          {bonusPrizes.map((prize, index) => (
            <Input
              key={index}
              value={bonusPrizes[index]}
              onChange={(e) => {
                let newPrizes = [...bonusPrizes];
                newPrizes[index] = e.target.value;
                setBonusPrizes(newPrizes);
              }}
              placeholder={`加碼獎 ${index + 1} 名稱`}
              className="border border-gray-400 p-2 rounded-lg w-full mt-1"
            />
          ))}
        </CardContent>
      </Card>
      <Button onClick={drawWinners} disabled={participants.length === 0} className="bg-green-600 hover:bg-green-700 text-white p-2 rounded-lg w-full">
        抽取主獎
      </Button>
      <Button onClick={drawBonusWinners} disabled={participants.length === 0} className="bg-yellow-500 hover:bg-yellow-600 text-white p-2 rounded-lg w-full">
        抽取加碼獎
      </Button>
      {winners.length > 0 && (
        <div className="mt-4">
          <h2 className="text-xl font-bold text-red-600">🎉 主獎得主 🎉</h2>
          {winners.map((winner, index) => (
            <motion.div key={index} initial={{ scale: 0 }} animate={{ scale: 1 }} className="text-lg font-bold text-gray-700 mt-2">
              {winner.name} - {winner.prize}
            </motion.div>
          ))}
        </div>
      )}
      {bonusWinners.length > 0 && (
        <div className="mt-4">
          <h2 className="text-lg font-bold text-orange-600">🎊 加碼獎得主 🎊</h2>
          {bonusWinners.map((winner, index) => (
            <motion.div key={index} initial={{ scale: 0 }} animate={{ scale: 1 }} className="text-lg font-bold text-gray-700 mt-2">
              {winner.name} - {winner.prize}
            </motion.div>
          ))}
        </div>
      )}
    </div>
  );
}
