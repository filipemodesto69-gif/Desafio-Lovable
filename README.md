# Desafio-Lovable

[Index.tsx](https://github.com/user-attachments/files/26098598/Index.tsx)
import { useState } from "react";
import { toast } from "sonner";
import BalanceHero from "@/components/BalanceHero";
import CommandBar from "@/components/CommandBar";
import TransactionList, { Transaction } from "@/components/TransactionList";
import MiniCharts from "@/components/MiniCharts";

const Index = () => {
  const [transactions, setTransactions] = useState<Transaction[]>([]);

  const parseTransaction = (text: string) => {
    const lower = text.toLowerCase();
    const isModCell = /iphone|celular|samsung|smartphone|apple/i.test(text);
    const type: "modcell" | "modloc" = isModCell ? "modcell" : "modloc";

    const valueMatch = text.match(/(\d[\d.,]*)/);
    const value = valueMatch ? `R$ ${valueMatch[1]}` : "R$ 0";

    const now = new Date();
    const time = `${now.getHours().toString().padStart(2, "0")}:${now.getMinutes().toString().padStart(2, "0")}`;

    const newTx: Transaction = {
      id: crypto.randomUUID(),
      text,
      type,
      value,
      time: `Hoje, ${time}`,
    };

    setTransactions((prev) => [newTx, ...prev]);
    toast.success("Tá lá! O lucro entrou na conta. 🚀", {
      description: "É o ModGroup crescendo!",
    });
  };

  return (
    <div className="min-h-screen bg-background">
      <div className="max-w-md mx-auto px-4 py-6 space-y-5">
        {/* Header */}
        <div className="flex items-center justify-between">
          <div>
            <p className="text-muted-foreground text-xs uppercase tracking-widest font-bold">
              ModFinance
            </p>
            <h2 className="text-lg font-bold tracking-tight text-foreground mt-0.5">
              Fala, Filipe! Pronto pro lucro? 💰
            </h2>
          </div>
          <div className="w-10 h-10 rounded-full bg-primary/20 border border-primary/30 flex items-center justify-center text-sm font-bold text-primary">
            F
          </div>
        </div>

        <BalanceHero />
        <CommandBar onProcess={parseTransaction} />
        <MiniCharts />
        <TransactionList transactions={transactions} />
      </div>
    </div>
  );
};

export default Index;

